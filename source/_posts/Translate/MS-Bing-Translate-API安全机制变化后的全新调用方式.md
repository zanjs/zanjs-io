---
title: MS Bing Translate API安全机制变化后的全新调用方式
date: 2017-06-01 15:34:31
tags:
  - translate
  - api
categories:
  - translate
---


前一阵子做一个自己的翻译网站时想加入一个Google Translate的网络翻译，
调查后发现Google现在已经开始收费，没办法，只好转微软的Bing Translate。
问题是，微软更新了API的调用方式，以前是只要有MSN账号，在bing的网站注册自己的开发者信息就可以通过安全key还是什么调用了，
而现在为了安全起见，微软同样需要你先去注册，
然后你想要调用API是需要先发送该注册账户到服务器，
微软会生成一个名为accessToken的key给你，之后你再拿这个key调用API，
此时才能真正的使用Bing Translate API。可见，旧的调用方式是一次web访问就可以，
而现在需要两次web访问，一次是为了获得accessToken，第二次才是调用API。
另外，需要注意的是，一次成功获得accessToken后有效期为10分钟，
在这10分钟内你调用Bing Translate API前不需要再重新获得新的accessToken。 
做这个功能花了点时间，因为官方的文档只是简单提了一下他们的安全验证方式改了，
之前的那种方式不再使用，问题是API还是旧的，里面的例子也是旧的，
在网上找了很多资料，基本也都是微软修改之前的实现方式，
自己摸爬滚打总算实现了，在这里介绍一下，方便其他人参考：

1. 首先当然是前台的js： 

```js
function BingTranslate() {  
    var options = {  
            url:'../doWebTranslate.do',  
            type:'POST',  
            dataType:'json',  
            success: BingTranslateSucceed  
        };  
    $.ajax(options);        
}
```

上面的代码中我省略了10分钟的判断，关键的其实就是发送ajax请求的这部分，
就是通过doWebTranslate.do这个action来获取accessToken的。
此次ajax相应成功后会执行BingTranslateSucceed函数，
该函数的作用才是访问Bing Translate API查单词的，
这里有两种方式访问，一种是直接通过js实现web访问，
另一种是通过java实现（当然需要另一个action了），简单起见，我用第一种： 

```js
/**  
* response function for Bing Translate request 
*/  
function BingTranslateSucceed(result) {  
    window.mycallback = function(searchResponse) {  
        preReqDoneFlg = true;  
            
        clearInterval(clock);  
        $("#bing-translate-result").text(searchResponse);  
        sec = 1;  
    };  
    if (!renewFlg) {  
        accessToken = result;  
    } else {  
        accessToken = encodeURIComponent(result.response.access_token);  
    }  
        
    var text = $('input[name=searchStr]').val();  
    var from = "en";  
    var to = "zh-CHS";  
    var s = document.createElement("script");  
    s.type = "text/javascript";  
    s.src = "http://api.microsofttranslator.com/V2/Ajax.svc/Translate?oncomplete=mycallback&appId=Bearer " + accessToken + "&from=" + from + "&to=" + to + "&text=" + text;  
    document.getElementsByTagName("head")[0].appendChild(s);  
}  
```

至此就实现了`Bing Translate API` 的调用，那么究竟是如何获取 `accessToken` 的呢？ 

获取 `accessToken` 的核心部分，这部分其实就是通过java的HttpPost访问网络数据了： 

```java
/**
 * request to Azure DataMarket to get access token
 * 
 * @author weishijie
 *
 */
public class AdmAuthentication
{
    private String clientId;
    private String clientSecret;

    public AdmAuthentication(String clientId, String clientSecret)
    {
        this.clientId = clientId;
        this.clientSecret = clientSecret;
    }

    public AdmAccessTokenVo GetAccessToken()
    {
        try {
			return HttpPost(AccessTokenGenConsts.DATA_MARKET_ACCESS_URI);
		} catch (Exception e) {
			LogUtil.log.info(e);
			e.printStackTrace();
			return null;
		}
    }

    /**
     * Make a http post request to the token service
     * 
     * @param DatamarketAccessUri https://datamarket.accesscontrol.windows.net/v2/OAuth2-13
     * @return AdmAccessTokenVo object
     * @throws Exception
     */
    private AdmAccessTokenVo HttpPost(String DatamarketAccessUri) throws Exception
    {
    	HttpClient httpClient = new DefaultHttpClient();
    	//建立HttpPost对象
    	HttpPost httppost = new HttpPost(DatamarketAccessUri);
    	//建立一个NameValuePair数组，用于存储欲传送的参数
    	List<NameValuePair> params=new ArrayList<NameValuePair>();
    	//添加参数
    	params.add(new BasicNameValuePair("client_id", this.clientId));
    	params.add(new BasicNameValuePair("client_secret", this.clientSecret));
    	params.add(new BasicNameValuePair("grant_type", AccessTokenGenConsts.GRANT_TYPE));
    	params.add(new BasicNameValuePair("scope", AccessTokenGenConsts.SCOPE));
    	//设置编码
    	httppost.setEntity(new UrlEncodedFormEntity(params, "UTF-8"));
    	//发送Post,并返回一个HttpResponse对象
    	HttpResponse response = httpClient.execute(httppost);
    	
    	String access_token = "";
        String token_type = "";
        String expires_in = "";
        String scope = "";
        //如果状态码为200,就是正常返回
    	if(response.getStatusLine().getStatusCode() == 200){
    		//如果是下载文件,可以用response.getEntity().getContent()返回InputStream
			String result=EntityUtils.toString(response.getEntity());
			JSONObject jsonObj = JSONObject.fromObject(result);
			
			access_token = jsonObj.getString("access_token");
	        token_type = jsonObj.getString("token_type");
	        expires_in = jsonObj.getString("expires_in");
	        scope = jsonObj.getString("scope");
		}
        
        AdmAccessTokenVo token = new AdmAccessTokenVo(access_token, token_type, expires_in, scope);
        return token;
    }
}

```


这个代码中需要5个常量，其中3个是固定的，还有两个则是自己在微软服务器上注册后得到的： 

```java
public class AccessTokenGenConsts {

	/** AccessTokenGen const */
	
	/** uri of Azure DataMarket */
	public static String DATA_MARKET_ACCESS_URI = "https://datamarket.accesscontrol.windows.net/v2/OAuth2-13";
	/** grant type of Token Request Input Parameters */
	public static String GRANT_TYPE = "client_credentials";
	/** scope of Token Request Input Parameters */
	public static String SCOPE = "http://api.microsofttranslator.com";
	/** client id of Token Request Input Parameters
	 * (registered in Azure DataMarket:https://datamarket.azure.com/developer/applications/)
	 */
	public static String CLIENT_ID = "XXX";
	/** client secret of Token Request Input Parameters
	 * (registered in Azure DataMarket:https://datamarket.azure.com/developer/applications/)
	 */
	public static String CLIENT_SECRET ="XXX";
	
}

```