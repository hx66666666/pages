# 《通信软件开发与应用》课程结业报告

本人做的是一个基于前后端交互的一个图书管理系统，前端的页面方面我使用的是Vue2框架进行搭建，配合使用前端热门组件Element-ui来进行一定的样式美化，并配合node环境下的Egg.js框架搭建一个简单的后台，同时使用axios来实现前后台的数据交互，然后通过后台调用MYSQL数据库，实现简单的增删改查功能。本系统拥有两种不同的用户，一种是普通的用户，可以注册使用该系统的查看书籍、借阅书籍、归还书籍、修改密码等，还有一种是管理人员，可以导入书籍、增加用户、修改用户、查看借阅书籍的信息等。



## 1、数据库情况

![image](https://user-images.githubusercontent.com/99119365/178130426-f921bd8a-9cc5-4091-bc00-1b1d8567b38c.png)

![image](https://user-images.githubusercontent.com/99119365/178130437-efe1a068-d5da-49b1-ab4a-772b9d873071.png)

![image](https://user-images.githubusercontent.com/99119365/178130442-080ce605-15c5-4c9b-8c46-28004fdea336.png)

![image](https://user-images.githubusercontent.com/99119365/178130446-cf79e43d-1ca4-4d9e-b71b-a8a7b21c1a45.png)



## 2、登录注册

首先进入本系统可以看到的是一个登录页面，唯一ID是标识用户的，当登录时，唯一ID不存在会直接提示用户不存在，这一步登录验证是直接通过axios向后台发起post请求，将登录人员的数据传递给后台，然后将登录人员数据与后台调用数据库中的人员数据进行循环比对，对比成功后就直接返回响应，然后前端代码.then()获取响应并判断响应类型后决定是否进行页面跳转，但是这里我没有设置路由守卫，所有直接通过不同页面的链接就可以直接访问，还有后台这里我们没有设置jwt验证，所有前端发送的请求都是不带token的，登录和注册安全存在问题安全存在问题。同时为了调用数据库，我们还要做一个数据的映射模型。

![image](https://user-images.githubusercontent.com/99119365/178130452-ee97ea26-e62a-4002-8df0-11ccf3523f70.png)

![image](https://user-images.githubusercontent.com/99119365/178130454-07231484-898c-4ab0-a67d-60cb2e6155fa.png)



普通用户不存在时，点击登录页面的注册按钮就可以跳转到注册页面，这是用前端路由来实现的，然后根据提示输入注册信息就可以注册成为该系统的普通用户，在数据库的用户表中我们就可以看到新添加的用户数据。

![image](https://user-images.githubusercontent.com/99119365/178130466-78dff8d2-3b8c-494a-ba74-0fffc3dae863.png)

![image](https://user-images.githubusercontent.com/99119365/178130468-8290d48b-bd0f-40a2-9aab-f0301d0c20b2.png)

![image](https://user-images.githubusercontent.com/99119365/178130474-4a80e259-61f0-476b-83e5-60e8c63737c5.png)



## 2、普通用户

然后我们就可以登录该系统进行使用了，进来之后就可以看到图书馆的首页，然后还有一个头部导航条，导航条的内容我写在pubuser.vue的页面中，然后，首页轮播图、图书查询、个人中心这些功能页面我都是写在组件上面的，父组件要使用子组件就需要先注册组件。
![image](https://user-images.githubusercontent.com/99119365/178130485-43495f16-0e58-42f3-8e7e-18005dd7895a.png)

![image](https://user-images.githubusercontent.com/99119365/178130487-d2760fa5-83c1-47e9-86bc-77efa7b30b3c.png)



### 2.1、图书查询功能

我们可以实现按照不同类别进行查询，如图书的名字、作者、类别等，这一步我直接调用书籍表中的书籍，进行循环遍历即可。

![image](https://user-images.githubusercontent.com/99119365/178130498-483ab242-c7b4-466d-9c46-460ab71757bd.png)

![image](https://user-images.githubusercontent.com/99119365/178130501-26403fc2-cb48-42b9-a7df-261c8bcba1bd.png)



本系统还可以进行图书的订阅和退订，当已经订阅时会显示以订阅，未订阅时会显示未订阅。这一步就是我将订阅的书籍存入另外一个表中，然后修改原书籍表中书籍状态，同样，退订的时候，将退订书籍从订阅表中删除，同时修改原书籍表中的书籍状态。



点击订阅后

![image](https://user-images.githubusercontent.com/99119365/178130509-a65cbea7-06f5-419e-bf8a-53ef281b89ce.png)

![image](https://user-images.githubusercontent.com/99119365/178130514-0b209b7b-30c6-4271-bf72-a0be978d4169.png)

![image](https://user-images.githubusercontent.com/99119365/178130521-2763465c-d675-4b73-9d75-42f5c7946783.png)


当你订阅一本书以后，你再次点击订，阅系统会提示你已经订阅
![image](https://user-images.githubusercontent.com/99119365/178130724-4e5456c2-ca51-47c6-8b8f-1007913bfeff.png)



点击退订后

![image](https://user-images.githubusercontent.com/99119365/178130535-cfaa3d2e-9951-4364-a7e4-30319b183365.png)

![image](https://user-images.githubusercontent.com/99119365/178130539-a07bf08d-220c-4c56-a86d-36772885d2ab.png)

![image](https://user-images.githubusercontent.com/99119365/178130542-a401c1a2-e973-45fa-802c-dfc598626f24.png)


当你已经退订一本书后，再次点击退订，系统会提示你没有订阅
![image](https://user-images.githubusercontent.com/99119365/178130753-4cc0d40c-b66e-4fee-97a0-e26bdad44ecc.png)



### 2.2、个人中心

这里可以看到我的登录人员信息，这里我使用了本地存储，在登录的时候将用户名和ID进行本地存储，其实我认为这样做是不合理的，因为，本地存储不安全，同时，如我在同一台浏览器上进行第二个用户的登录，就会覆盖掉我前面一个人员的信息。

![image](https://user-images.githubusercontent.com/99119365/178130554-9303d8bc-a6c1-4ea8-812e-e2b6f0ab60a9.png)

![image](https://user-images.githubusercontent.com/99119365/178130555-3e97817c-cef8-4f32-b5a5-f7e7d9656ceb.png)

![image](https://user-images.githubusercontent.com/99119365/178130557-7ef17049-504c-4952-9a52-5456476f655d.png)



这里我们可以修改自己的密码

![image](https://user-images.githubusercontent.com/99119365/178130565-c619a9f8-4394-4a75-9bab-026a4b0c5a6d.png)

![image](https://user-images.githubusercontent.com/99119365/178130566-8b8ad14f-7432-400b-a1fc-461e0e2d1bfa.png)



### 2.3、退出界面

直接就退出到登录页面，这里我在退出时设置了清空本地存储。

![image](https://user-images.githubusercontent.com/99119365/178130573-7239f0af-8baa-40ef-adaa-9de808c7e33b.png)




## 3、系统管理员

本系统用于系统管理员，系统管理员登录也是如上的登录页面，然后我们进入到主页，我们可以看到本系统有用图书管理，借阅书籍查询，人员管理等。

![image](https://user-images.githubusercontent.com/99119365/178130580-d786cf40-730b-45f0-905c-873767746949.png)



### 3.1、图书管理

这里我们可以增加和删除书籍，包括书籍的名称、书籍的位置、书籍的作者、借阅状态。

![image](https://user-images.githubusercontent.com/99119365/178130588-e5b34e1a-84c4-4dea-8e14-c5ace12a12f0.png)

![image](https://user-images.githubusercontent.com/99119365/178130593-576a57b1-aa90-469d-b96d-965642d758a8.png)

![image](https://user-images.githubusercontent.com/99119365/178130606-da97f697-2ac8-4406-bbe4-dae475a1e0b4.png)

![image](https://user-images.githubusercontent.com/99119365/178130610-fe3a5fd8-83ef-485f-8316-77f5eb422fa6.png)



点击删除书籍后

![image](https://user-images.githubusercontent.com/99119365/178130614-cb844b8e-06ad-4302-a15c-cc3b67122948.png)

![image](https://user-images.githubusercontent.com/99119365/178130621-641c6872-501e-49da-aa0c-5e12e7fe51c8.png)

![image](https://user-images.githubusercontent.com/99119365/178130625-38bb2de1-0097-4047-a3cc-2959c6778f71.png)



### 3.2、以借阅的图书

我可以看到借阅者的ID，以及图书的详情等。

![image](https://user-images.githubusercontent.com/99119365/178130629-3e24cd69-4381-4f57-b322-12dee57d133e.png)



### 3.3、人员管理中心

这里可以新增加用户，也可以增加管理员，当然也可以实现对用户的删除

![image](https://user-images.githubusercontent.com/99119365/178130636-05643eff-8478-47d0-a6d5-e7b84aace08d.png)

![image](https://user-images.githubusercontent.com/99119365/178130642-c13bafbc-1517-4679-9be2-262b3b5dacdd.png)

![image](https://user-images.githubusercontent.com/99119365/178130651-92845fb9-67fc-4a5c-a5f4-6b5953931035.png)

![image](https://user-images.githubusercontent.com/99119365/178130655-caa936d4-1bff-4b6b-a19a-bd36ac3ed5e4.png)



点击删除后、可以删除该用户

![image](https://user-images.githubusercontent.com/99119365/178130659-bd408518-1c20-4a24-9442-e17ee9e4cbbe.png)

![image](https://user-images.githubusercontent.com/99119365/178130665-65cde48e-e987-45f7-8b96-c3786e081ec5.png)

![image](https://user-images.githubusercontent.com/99119365/178130671-bf7a7e28-1d87-4f14-9bfa-7ccde41039f9.png)



# 4、总结

本次是我第一次编写前后端结合的项目，在本次开发中，我遇到了许多的问题，比如，我并不太熟悉后台开发，我的后台开发是在网上学的一个速成的教学，所有我遇到了许多的问题，比如说，后台调用数据库，然后要写一个数据映射模型，那一步我跟着Egg.js的官方教程走，但是我感觉那个教程有点省略，大量JSON数据导入数据库时，那个实例代码写得比较模糊，最后还是试探出来的，然后就是前后端的交互，第一个接触后端，对应许多接口的写法还不是很熟悉。前端方面，我Vue也算是初学，我看了Vue那个的官方文档，按照那个教程来，Element-ui方面，我也是仅仅会简单复制官方文档提供的组件，虽然本项目比较简陋，而且BUG也比较多，但终究是我第一次写，我相信通过不断地学习，我也能够更加深入了解这些技术。


# 注：
前端：vue2
后端：Egg.js

运行：
进入前端demo文件，先 npm install 下载包，然后 npm  run serve
进入后端server文件，先 npm install 下载包，然后 npm  run dev

数据库：mysql
库：mylib
具体可以在：server\config\config.default.js文件夹下查看
dialect: 'mysql',
database: 'mylib',
host: 'localhost',
port: 3306,
username: 'root',
password: '123456',
timezone: '+08:00',

