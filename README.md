# 包含注释的 bottle.py
使用 2019-06-01 的 master 版本，commit ID 为 [a454029f6e8a087e5cb570eb6ee36c2087d26e4d](https://github.com/bottlepy/bottle/blob/a454029f6e8a087e5cb570eb6ee36c2087d26e4d/bottle.py).

## 划重点
### Route
对路由规则进行封装；在注册时通过**插件**对原始会回调函数进行修饰。

### Router
为了提升性能，将所有路由规则编译成正则后，会将所有 group 去除，每 99 条规则放在一起进行批量匹配；  
匹配到某条规则后，再用带组名的正则重新匹配一次，以获得组名对应的值。

