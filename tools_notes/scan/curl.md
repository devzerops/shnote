- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## curl이란

다양한 프로토콜로 상호작용하기 좋은 만능도구이다. 파일을 다운받고, 값을 보내고 받아고오, API를 요청하고 ftp를 쓰는 등 행위도 가능하고  
html 자체를 크롤링할수있는 등도 가능하기에 두루두루 쓰이는 도구이다.  


## option

* -X or --request: Specifies the request method to use. For example, to send a GET request, use -X GET.
* -u or --user: Sends a username and password for basic authentication. For example, to send a username and password, use -u "username:password".
* -s or --silent: Runs
* -o or --output: Saves the response to a file. For example, to save the response to a file named "response.txt", use -o response.txt.
* -L or --location: Follows redirects.
* -I or --head: Sends a HEAD request and shows only the headers of the response.
* -H or --header: Sends a custom header in the request. For example, to send a "Content-Type" header, use -H "Content-Type: application/json".
* -d or --data: Sends data in the request body. For example, to send a POST request with data, use -d "name=value".
* -b or --cookie: Sends a cookie in the request. For example, to send a cookie named "sessionid" with a value of "abc123", use -b "sessionid=abc123".


## example 

``` bash
curl -X GET https://www.example.com
```

``` bash
curl -X POST -d "name=value" https://www.example.com/submit
```

``` bash
curl -X GET -H "Content-Type: application/json" https://www.example.com/data
```

``` bash
curl -X GET -u "username:password" https://www.example.com/secure
```

``` bash
curl -o response.txt https://www.example.com
```

``` bash
curl -I https://www.example.com
```

``` bash
curl -L https://www.example.com/
```

``` bash
curl -b "sessionid=abc123" https://www.example.com
```

``` bash
curl -s https://www.example.com
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
