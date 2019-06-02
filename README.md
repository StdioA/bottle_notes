# 包含注释的 bottle.py
使用 2019-06-01 的 master 版本，commit ID 为 [a454029f6e8a087e5cb570eb6ee36c2087d26e4d](https://github.com/bottlepy/bottle/blob/a454029f6e8a087e5cb570eb6ee36c2087d26e4d/bottle.py).

## 划重点
### Route
对路由规则进行封装；在注册时通过**插件**对原始会回调函数进行修饰。

### Router
为了提升性能，将所有路由规则编译成正则后，会将所有 group 去除，每 99 条规则放在一起进行批量匹配；  
匹配到某条规则后，再用带组名的正则重新匹配一次，以获得组名对应的值。

### Bottle
Bottle app 所做的事：
1. 管理配置（ConfigDict）
2. 管理路由（Router）
3. 实现 WSGI 接口（wsgi）
    1. 填充 environ & request & response 的上下文
    2. 根据路由规则匹配处理函数
    3. 调用函数收到响应（函数返回值，或以异常形式抛出的 HTTPResponse）
    4. 将各种形式的响应转换为符合 WSGI 规范的 body list，同时填充 response
    5. `start_response`，返回 body，完成请求。
4. 管理插件（Plugin）
5. 实现 Hook（app 内部不会用到，但用户可以在内置钩子中添加 listener）

### Request
提供请求的各种解析，比如 cookie, query, body, file 等等。

读取请求体时，如果内容不多，则使用 `BytesIO` 存储在内存中，如果很大，就会用 `TemporaryFile`.

### Response
提供响应的设置，比如 status, header, cookies 和 body。  
有多重继承，方便使用。
