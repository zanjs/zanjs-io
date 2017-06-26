---
title: laravel 5.4 JWT 多用户认证
date: 2017-06-26 08:50:42
tags:
  - laravel
  - jwt
categories:
  - laravel
---


1. 在 composer.json 文件的 require 中添加如下行来引入 JWT-Auth

```
"require": {
    ...
    "tymon/jwt-auth":"1.0.0-beta.3"
    ...
}
```

执行 `composer update`


2. comfig/app.php 配置

`providers` 下添加


```php
Tymon\JWTAuth\Providers\LaravelServiceProvider::class,
```

`aliases` 下添加

```php
'JWTAuth' => Tymon\JWTAuth\Facades\JWTAuth::class,
'JWTFactory' => Tymon\JWTAuth\Facades\JWTFactory::class,
```

3. 生成 `jwt.php` 文件

在 cmd 窗口执行命令

```sh
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"
```


会在 `config` 文件夹下生成 `jwt.php`  配置文件

接着生成 `jwt` 密钥

```sh
 php artisan jwt:secret
```


4. config/auth.php 设置相关参数

我这里需要添加一个微信用户表

```php
'guards' => [
    ...
    'api' => [
            'driver' => 'jwt',
            'provider' => 'we_users',
        ],
],

'providers' => [
    ...
    'we_users' => [
        'driver' => 'eloquent',
        'model' => App\WeUser::class,
    ],
],
```


5. 创建 WeUser 模型

```sh
php artisan make:model WeUser -m
```

编辑 WeUser.php

```php

use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Tymon\JWTAuth\Contracts\JWTSubject as AuthenticatableUserContract;
class WeUser extends Authenticatable implements AuthenticatableUserContract
{
    use Notifiable;
    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [
        'name', 'email', 'password',
    ];
    /**
     * The attributes that should be hidden for arrays.
     *
     * @var array
     */
    protected $hidden = [
        'password', 'remember_token',
    ];
    /**
     * @return mixed
     */
    public function getJWTIdentifier()
    {
        return $this->getKey(); // Eloquent model method
    }
    /**
     * @return array
     */
    public function getJWTCustomClaims()
    {
        return [];
    }
}
```


6. route.php 中添加登录路由


```php
Route::post('/login','WeUserController@login');
```

7. 创建 `WeUser` 控制器

```sh
php artisan make:controller WeUserController
```

8. 编辑微信用户控制器 WeUserController.php

```php
/**
* 微信用户自动登陆
* @param Request $request
* @return \Illuminate\Http\JsonResponse
*/
public function login(Request $request)
{
    $hasUser = WeUser::Where('email', $request->email)->first();
    if(!$hasUser){
        WeUser::create([
            'name' => $request->name,
            'email' => $request->email,
            'password' => bcrypt($request->password),
        ]);
    }
    $credentials=[
        'email' => $request->email,
        'password'  => $request->password,
    ];
    try {
        if (! $token = Auth::guard($this->guard)->attempt($credentials)) {
            return response()->json(['error' => 'invalid_credentials'], 401);
        }
    } catch (JWTException $e) {
        return response()->json(['error' => 'could_not_create_token'], 500);
    }
    return response()->json(compact('token'));
}

```

