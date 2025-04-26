微信服务器端，c++实现，整体逻辑如下：
![image](https://github.com/1li2yang3/WeiXing_Server/blob/main/%E6%9E%B6%E6%9E%84%E5%9B%BE.png)
用户点击注册，客户端GateServer发送用户邮箱，GateServer将邮箱转发给VarifyServer，VarifyServer生成验证码并向用户邮箱转发，同时将验证码回传给GateServer，GateServer回传给客户端，用户输入验证码则进行核对，邮箱和验证码会储存在Redis中。
![image](https://github.com/1li2yang3/WeiXing_Server/blob/main/%E6%B3%A8%E5%86%8C.png)
用户点击登录，客户端向GateServer发送请求，GateServer转发给StatusServer，StatusServer判断聊天服务器负载并生成用户对应的token，StatusServer将负载最小的聊天服务器ip地址和用户token回传给GateServer，且向对应聊天服务器发送一份token用于校验，客户端收到ip后向对应服务器发起TCP请求并提供token进行校验，聊天服务器校验正确后确认连接。
![image](https://github.com/1li2yang3/WeiXing_Server/blob/main/%E7%99%BB%E5%BD%95.png)
用户A向用户B发送消息，若两人在同一聊天服务器下，则服务器转发消息，否则转发给StatusServer查找用户B所在服务器进行转发。
![image](https://github.com/1li2yang3/WeiXing_Server/blob/main/%E9%80%9A%E8%AE%AF.png)
项目涉及iocontex、mysqlconnection、Redisconnection、grpcconnectin等池保证并发性，采用分布式架构解决高并发下单个服务器瓶颈


