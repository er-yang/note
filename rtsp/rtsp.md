### 地址

rtsp和rtspu两种URL scheme都可以用于rtsp协议的通信，rtspu指通过udp来进行通信传输

```
   rtsp_URL  =   ( "rtsp:" | "rtspu:" )
                 "//" host [ ":" port ] [ abs_path ]
                 
    /** abs_path 绝对路径其解释权由rtspServer决定 */              
```

### 消息体

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

Method有
