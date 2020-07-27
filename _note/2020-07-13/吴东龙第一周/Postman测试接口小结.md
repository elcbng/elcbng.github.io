## Postman测试接口小结

- **测试准备**

  接口文档、Postman测试工具

- **测试用例必备字段**

  URL、请求方式、请求参数、预期结果

  

- ##### 第一步

![1594630376237](C:\Users\25469\AppData\Roaming\Typora\typora-user-images\1594630376237.png)

 	选择请求方式、输入想要测试的接口



- #### 第二步

  ![1594630455370](C:\Users\25469\AppData\Roaming\Typora\typora-user-images\1594630455370.png)

  

  设置好的请求格式，此处为Post请求格式：Content-Type，application/json

  

- #### 第三步

  ![1594630607100](C:\Users\25469\AppData\Roaming\Typora\typora-user-images\1594630607100.png)

  选择 raw格式，输入请求参数  (json格式)

- #### 最后

  ![](C:\Users\25469\AppData\Roaming\Typora\typora-user-images\1594631276921.png)

  ![1594630767011](C:\Users\25469\AppData\Roaming\Typora\typora-user-images\1594630767011.png)

  

  在**Response**处可以看到返回结果

  

  

  

- ## 用表单来进行传参

  方法与json格式传参类似，请求参数选择 **Params**  ,区别于 **Body**

  请求头处的**Value**选择**application/x-www-from-urlencoded**