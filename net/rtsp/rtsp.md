# RTSP

[rfc](https://tools.ietf.org/html/rfc2326)

### 地址

rtsp和rtspu两种URL scheme都可以用于rtsp协议的通信，rtspu指通过udp来进行通信传输，其采用默认端口554

```
   rtsp_URL  =   ( "rtsp:" | "rtspu:" )
                 "//" host [ ":" port ] [ abs_path ]
                 
    /** abs_path 绝对路径其解释权由rtspServer决定 */              
```

### 消息体

消息可分为header和body两部分

rtsp是基于文档传输的协议，使用的是ISO 10646字符集utf-8编码，每一行有CRLF符标志结束，虽说文本协议的处理效率低，官方文档给出的解释是，参数个数少和发送命令的频率很低，所以可以不用考虑这微小的消耗

第一个空行标志着header的结束

#### RequestMessage

 [see detail](https://tools.ietf.org/html/rfc2326#section-6) 

```
       Request      =       Request-Line          ; Section 6.1
                    *(      general-header        ; Section 5
                    |       request-header        ; Section 6.2
                    |       entity-header )       ; Section 8.1
                            CRLF
                            [ message-body ]      ; Section 4.3
                            
       Request-Line = Method SP Request-URI SP RTSP-Version CRLF
       
          Method         =  "DESCRIBE"              ; Section 10.2
                  |         "ANNOUNCE"              ; Section 10.3
                  |         "GET_PARAMETER"         ; Section 10.8
                  |         "OPTIONS"               ; Section 10.1
                  |         "PAUSE"                 ; Section 10.6
                  |         "PLAY"                  ; Section 10.5
                  |         "RECORD"                ; Section 10.11
                  |         "REDIRECT"              ; Section 10.10
                  |         "SETUP"                 ; Section 10.4
                  |         "SET_PARAMETER"         ; Section 10.9
                  |         "TEARDOWN"              ; Section 10.7
                  |         extension-method

       extension-method = token
       
       RTSP-Version = "RTSP" "/" 1*DIGIT "." 1*DIGIT
       request-header  =          Accept                   ; Section 12.1
                  |          Accept-Encoding          ; Section 12.2
                  |          Accept-Language          ; Section 12.3
                  |          Authorization            ; Section 12.5
                  |          From                     ; Section 12.20
                  |          If-Modified-Since        ; Section 12.23
                  |          Range                    ; Section 12.29
                  |          Referer                  ; Section 12.30
                  |          User-Agent

```

#### ResponseMessage

```
     Response    =     Status-Line         ; Section 7.1
                 *(    general-header      ; Section 5
                 |     response-header     ; Section 7.1.2
                 |     entity-header )     ; Section 8.1
                       CRLF
                       [ message-body ]    ; Section 4.3
                       
         Status-Line =   RTSP-Version SP Status-Code SP Reason-Phrase CRLF
         response-header  =     Location             ; Section 12.25
                          |     Proxy-Authenticate   ; Section 12.26
                          |     Public               ; Section 12.28
                          |     Retry-After          ; Section 12.31
                          |     Server               ; Section 12.36
                          |     Vary                 ; Section 12.42
                          |     WWW-Authenticate 
```

statusCode 状态吗与http类似，1XX（information），2XX（success），3XX（Redirection），4xx（Clinet error），5XX（Server error）[状态码](https://tools.ietf.org/html/rfc2326#section-7.1.1)

#### 通用头部

##### entity-header

描述的是body的相关元信息

```
    entity-header       =    Allow               ; Section 12.4
                         |    Content-Base        ; Section 12.11
                         |    Content-Encoding    ; Section 12.12
                         |    Content-Language    ; Section 12.13
                         |    Content-Length      ; Section 12.14
                         |    Content-Location    ; Section 12.15
                         |    Content-Type        ; Section 12.16
                         |    Expires             ; Section 12.19
                         |    Last-Modified       ; Section 12.24
                         |    extension-header
     extension-header    =    message-header
```

##### Message-length

表示的是body的长度不包括header，如果未携带content-length则默认为0

##### CSeq

每个请求头部都必须要带的一个序列号，在不同的请求中，其值做递增加1处理，同时，每个请求对应的response中，会携带相同的序列号，对与一个重传的请求必须要保持原始请求Cseq相同的值