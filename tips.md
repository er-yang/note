### Chrome

* Window.open在对传入参数为null时存在行为差异
* XmlHttpRequest 在POST请求下，Content-Type中charset utf-8会自动转成大写

### W3C

W3C规定了一些[forbidden header](https://developer.mozilla.org/en-US/docs/Glossary/Forbidden_header_name) ，不能通过代码取修改
### CORE
在跨域访问时受同源策略影响，header中的只能获取[simple response headers](https://developer.mozilla.org/en-US/docs/Glossary/Simple_response_header) ：
* Cache-Control
* Content-Language
* Content-Type
* Expires
* Last-Modified
* Pragma

如果需要获取其他response header字段，需要头部返回Access-Control-Expose-Headers： 来允许获取