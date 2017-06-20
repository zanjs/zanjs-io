---
title: Selenium 3 python 2 判断元素是否存在
date: 2017-06-20 12:05:53
tags:
  - selenium
  - python
categories:
  - selenium
---

## 捕获异常方法

1. 如果没找到元素会抛异常，返回False

2. 如果找到元素就返回Ture

3. 但是这个方法有个弊端，如果页面上存在多个一样元素，也会返回Ture的（也就是说只要页面上有元素就返回Ture，不管几个）

```python
try:
    driver.find_elements_by_css_selector('css')
    return True
except
    return False

```

## 参考代码

```py
# coding:utf-8
from selenium import webdriver

driver = webdriver.Firefox()
driver.implicitly_wait(10)
driver.get("https://www.baidu.com")
def is_element_exist(css):
    s = driver.find_elements_by_css_selector(css_selector=css)
    if len(s) == 0:
        print "元素未找到:%s"%css
        return False
    elif len(s) == 1:
        return True
    else:
        print "找到%s个元素：%s"%(len(s),css)
        return False

# 判断页面上有无id为kw的元素
if is_element_exist("#kw"):
    driver.find_element_by_id("kw").send_keys("zanjs")
# 判断页面有无标签为input元素
if is_element_exist("input"):
    driver.find_element_by_tag_name("input").send_keys("zanjs")
# 判断页面有无id为xxx的元素
if is_element_exist("xxx"):
    driver.find_element_by_id("xxx").send_keys("zanjs")

def isElementExist(css):
    try:
        driver.find_element_by_css_selector(css)
        return True
    except:
        return False

print isElementExist("#xxx")
```