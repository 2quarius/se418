# 简介

### 主要功能
简单的注册、登录后进入`/wordladder`并将起始单词（start）和目标单词（dest）写入地址，例如:
`/wordladder/hello/world`。
即可获知hello->...->world的word ladder。
### 技术栈
- Spring Boot
- Spring Security
- Unit Test
### 使用步骤
1、执行 `mvn spring-boot:run`

2、【未登录】获取个人信息
  ```
  curl -X GET http://localhost:8080/wordladder
  
  # 结果
  {"code":"401","message":"authenticate fail"}
  ```
3、【未登录】注册账号
  ```
  curl -X POST \
    http://localhost:8080/register \
    -H 'Content-Type: application/json' \
    -d '{
  	"username":"ruirui",
  	"password":"renrui",
  	"nickname":"rendie"
  }'
  
  # 结果
  {"code": "0","message": "success","result": "注册成功"}
  ```
4、登录账号
  ```
  curl -v -X POST \
    http://localhost:8080/login \
    -H 'Content-Type: application/json' \
    -d '{
          "username":"ruirui",
          "password":"renrui"
  }'
  
  # 结果
  Note: Unnecessary use of -X or --request, POST is already inferred.
  *   Trying ::1...
  * TCP_NODELAY set
  * Connected to localhost (::1) port 8080 (#0)
  > POST /login HTTP/1.1
  > Host: localhost:8080
  > User-Agent: curl/7.54.0
  > Accept: */*
  > Content-Type: application/json
  > Content-Length: 58
  >
  * upload completely sent off: 58 out of 58 bytes
  < HTTP/1.1 200
  < X-Content-Type-Options: nosniff
  < X-XSS-Protection: 1; mode=block
  < Cache-Control: no-cache, no-store, max-age=0, must-revalidate
  < Pragma: no-cache
  < Expires: 0
  < X-Frame-Options: DENY
  < Set-Cookie: JSESSIONID=8A883B9793B1C67F2055B8F13D7538FF; Path=/; HttpOnly
  < Content-Type: application/json;charset=UTF-8
  < Content-Length: 56
  < Date: Sat, 06 Apr 2019 08:38:29 GMT
  <
  * Connection #0 to host localhost left intact
  {"code":"0","message":"success","result":"登录成功"}
  ```
> 登录成功，并返回 SESSION 信息：`Set-Cookie: JSESSIONID=8A883B9793B1C67F2055B8F13D7538FF; Path=/; HttpOnly`

5、【已登录】使用wordladder
  ```
  curl -v -X POST \
    http://localhost:8080/wordladder/hello/world \
    -H 'Cookie: JSESSIONID=8E46D6D03DA11D89FE507F2F2462A21E'
    
  # 结果
  *   Trying ::1...
  * TCP_NODELAY set
  * Connected to localhost (::1) port 8080 (#0)
  > POST /wordladder/hello/world HTTP/1.1
  > Host: localhost:8080
  > User-Agent: curl/7.54.0
  > Accept: */*
  > Cookie: JSESSIONID=8E46D6D03DA11D89FE507F2F2462A21E
  >
  < HTTP/1.1 200
  < X-Content-Type-Options: nosniff
  < X-XSS-Protection: 1; mode=block
  < Cache-Control: no-cache, no-store, max-age=0, must-revalidate
  < Pragma: no-cache
  < Expires: 0
  < X-Frame-Options: DENY
  < Content-Type: application/json;charset=UTF-8
  < Transfer-Encoding: chunked
  < Date: Sat, 06 Apr 2019 08:41:38 GMT
  <
  * Connection #0 to host localhost left intact
  {"id":1,"start":"hello","dest":"world","answer":"Found Ladder:hello->hells->wells->weals->weald->woald->world\n"}

```
6、【已登录】退出登录
```
curl -X GET http://localhost:8080/logout

# 结果
{"code":"0","message":"success","result":"退出成功"}
```
7、【已登录】调用actuator监控信息
```$xslt
curl -v -X GET \
  http://localhost:8080/management \
  -H 'Cookie: JSESSIONID=8E46D6D03DA11D89FE507F2F2462A21E'
  
#结果
*   Trying ::1...
* TCP_NODELAY set
* Connected to localhost (::1) port 8080 (#0)
> GET /management HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.54.0
> Accept: */*
> Cookie: JSESSIONID=B84AE0C87D78D272AC2F07AED029FADF
>
< HTTP/1.1 200
< X-Content-Type-Options: nosniff
< X-XSS-Protection: 1; mode=block
< Cache-Control: no-cache, no-store, max-age=0, must-revalidate
< Pragma: no-cache
< Expires: 0
< X-Frame-Options: DENY
< Content-Type: application/vnd.spring-boot.actuator.v2+json;charset=UTF-8
< Transfer-Encoding: chunked
< Date: Sat, 13 Apr 2019 14:21:09 GMT
<
* Connection #0 to host localhost left intact
{"_links":{"self":{"href":"http://localhost:8080/management","templated":false},"auditevents":{"href":"http://localhost:8080/management/auditevents","templated":false},"beans":{"href":"http://localhost:8080/management/beans","templated":false},"caches-cache":{"href":"http://localhost:8080/management/caches/{cache}","templated":true},"caches":{"href":"http://localhost:8080/management/caches","templated":false},"health":{"href":"http://localhost:8080/management/health","templated":false},"health-component":{"href":"http://localhost:8080/management/health/{component}","templated":true},"health-component-instance":{"href":"http://localhost:8080/management/health/{component}/{instance}","templated":true},"conditions":{"href":"http://localhost:8080/management/conditions","templated":false},"configprops":{"href":"http://localhost:8080/management/configprops","templated":false},"env":{"href":"http://localhost:8080/management/env","templated":false},"env-toMatch":{"href":"http://localhost:8080/management/env/{toMatch}","templated":true},"info":{"href":"http://localhost:8080/management/info","templated":false},"loggers":{"href":"http://localhost:8080/management/loggers","templated":false},"loggers-name":{"href":"http://localhost:8080/management/loggers/{name}","templated":true},"heapdump":{"href":"http://localhost:8080/management/heapdump","templated":false},"threaddump":{"href":"http://localhost:8080/management/threaddump","templated":false},"metrics":{"href":"http://localhost:8080/management/metrics","templated":false},"metrics-requiredMetricName":{"href":"http://localhost:8080/management/metrics/{requiredMetricName}","templated":true},"scheduledtasks":{"href":"http://localhost:8080/management/scheduledtasks","templated":false},"httptrace":{"href":"http://localhost:8080/management/httptrace","templated":false},"mappings":{"href":"http://localhost:8080/management/mappings","templated":false}}}%
```
- 格式化结果
```
 {
  	"_links": {
  		"self": {
  			"href": "http://localhost:8080/management",
  			"templated": false
  		},
  		"auditevents": {
  			"href": "http://localhost:8080/management/auditevents",
  			"templated": false
  		},
  		"beans": {
  			"href": "http://localhost:8080/management/beans",
  			"templated": false
  		},
  		"caches-cache": {
  			"href": "http://localhost:8080/management/caches/{cache}",
  			"templated": true
  		},
  		"caches": {
  			"href": "http://localhost:8080/management/caches",
  			"templated": false
  		},
  		"health": {
  			"href": "http://localhost:8080/management/health",
  			"templated": false
  		},
  		"health-component": {
  			"href": "http://localhost:8080/management/health/{component}",
  			"templated": true
  		},
  		"health-component-instance": {
  			"href": "http://localhost:8080/management/health/{component}/{instance}",
  			"templated": true
  		},
  		"conditions": {
  			"href": "http://localhost:8080/management/conditions",
  			"templated": false
  		},
  		"configprops": {
  			"href": "http://localhost:8080/management/configprops",
  			"templated": false
  		},
  		"env": {
  			"href": "http://localhost:8080/management/env",
  			"templated": false
  		},
  		"env-toMatch": {
  			"href": "http://localhost:8080/management/env/{toMatch}",
  			"templated": true
  		},
  		"info": {
  			"href": "http://localhost:8080/management/info",
  			"templated": false
  		},
  		"loggers": {
  			"href": "http://localhost:8080/management/loggers",
  			"templated": false
  		},
  		"loggers-name": {
  			"href": "http://localhost:8080/management/loggers/{name}",
  			"templated": true
  		},
  		"heapdump": {
  			"href": "http://localhost:8080/management/heapdump",
  			"templated": false
  		},
  		"threaddump": {
  			"href": "http://localhost:8080/management/threaddump",
  			"templated": false
  		},
  		"metrics": {
  			"href": "http://localhost:8080/management/metrics",
  			"templated": false
  		},
  		"metrics-requiredMetricName": {
  			"href": "http://localhost:8080/management/metrics/{requiredMetricName}",
  			"templated": true
  		},
  		"scheduledtasks": {
  			"href": "http://localhost:8080/management/scheduledtasks",
  			"templated": false
  		},
  		"httptrace": {
  			"href": "http://localhost:8080/management/httptrace",
  			"templated": false
  		},
  		"mappings": {
  			"href": "http://localhost:8080/management/mappings",
  			"templated": false
  		}
  	}
  } %
```