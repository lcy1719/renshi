
> **代码提供有偿远程调试，29包启动成功！不管你有没有运行环境，哪怕你是刚买的新电脑，也包启动运行成功！提供有偿讲解，有不懂的地方随便问！问到会为止！**
## 源码获取（4.9元包含代码+数据库+有的送报告文档具体扫码询问，代码截图在下面）：
![微信图片_20241218164848](https://github.com/user-attachments/assets/646b2784-afb8-47ee-a4d4-5ccc9f96b331)

**基于SpringBoot+Vue实现的人事管理系统采用前后端分流的架构方式，设计了管理员、员工两种角色，实现了员工管理、部门管理、员工请假管理、员工加班管理、员工工资管理、员工考勤管理、招聘信息管理、员工培训管理、员工详情管理等功能模块**

## 技术选型
开发工具：idea2020.3+Webstorm2020.3(其他开发工具也可以)
运行环境：jdk1.8+maven3.6.0+MySQL5.7+nodejs14.21
服务端技术：Springboot+Mybatis-plus
前端技术：html+css+Layui+jQuery+bootstrap+Vue+axios+Element-UI
## 成果展示
文档展示
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d965c3b8e9434c098c76391b0498436e.png)

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/5422e9b84fb5471eb2fb00b92abcf31c.png)
用户登录
![用户登录](https://i-blog.csdnimg.cn/direct/03e2a73322604a64a16bc1b0faf72680.png)
员工管理
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/20a96f22822e4c53a3a9708353d91fe0.png)
员工详情
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/3b5dd8f8f15e44cb95e1877e1c82a974.png)
发放工资
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2676b86202ea46fba2b8a97e7adb958e.png)
部门管理
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/70ed6ce8b6c9430fa51688c7c55e4256.png)
员工考勤管理
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/93c1cd856ca2453996c3ff9975400333.png)
请假申请管理
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/6d4a132d6fdf4a80854670a6bbfe3a3a.png)
加班申请管理
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/fed9eae9591c44d48d1427e9399f0329.png)
员工工资管理
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b7e5166f190e4e49be7400fd0e9c933d.png)
招聘计划管理
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/745d4de3900347df932bbd5d129fc7ec.png)
员工培训管理
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2e73d15a01484a5aacd7c7d486a0e3a7.png)
员工详情管理
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f453d57994db403694507d13ec5b66c9.png)
目录结构展示
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/bce2fcc67281479c8c0855cc0d50a782.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/576b061ef1c54c11af5770bcdb6f373e.png)


部分源码展示

```java
package com.controller;


import java.util.Arrays;
import java.util.Calendar;
import java.util.Date;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

import com.annotation.IgnoreAuth;
import com.baomidou.mybatisplus.mapper.EntityWrapper;
import com.entity.TokenEntity;
import com.entity.UserEntity;
import com.service.TokenService;
import com.service.UserService;
import com.utils.CommonUtil;
import com.utils.MPUtil;
import com.utils.PageUtils;
import com.utils.R;
import com.utils.ValidatorUtils;

/**
 * 登录相关
 */
@RequestMapping("users")
@RestController
public class UserController{
 
 @Autowired
 private UserService userService;
 
 @Autowired
 private TokenService tokenService;

 /**
  * 登录
  */
 @IgnoreAuth
 @PostMapping(value = "/login")
 public R login(String username, String password, String captcha, HttpServletRequest request) {
  UserEntity user = userService.selectOne(new EntityWrapper<UserEntity>().eq("username", username));
  if(user==null || !user.getPassword().equals(password)) {
   return R.error("账号或密码不正确");
  }
  String token = tokenService.generateToken(user.getId(),username, "users", user.getRole());
  return R.ok().put("token", token);
 }
 
 /**
  * 注册
  */
 @IgnoreAuth
 @PostMapping(value = "/register")
 public R register(@RequestBody UserEntity user){
  // ValidatorUtils.validateEntity(user);
     if(userService.selectOne(new EntityWrapper<UserEntity>().eq("username", user.getUsername())) !=null) {
      return R.error("用户已存在");
     }
        userService.insert(user);
        return R.ok();
    }

 /**
  * 退出
  */
 @GetMapping(value = "logout")
 public R logout(HttpServletRequest request) {
  request.getSession().invalidate();
  return R.ok("退出成功");
 }
 
 /**
     * 密码重置
     */
    @IgnoreAuth
 @RequestMapping(value = "/resetPass")
    public R resetPass(String username, HttpServletRequest request){
     UserEntity user = userService.selectOne(new EntityWrapper<UserEntity>().eq("username", username));
     if(user==null) {
      return R.error("账号不存在");
     }
     user.setPassword("123456");
        userService.update(user,null);
        return R.ok("密码已重置为：123456");
    }
}
```
## 为什么选择我？

>  博主本身也是从大学走出来的，所有编程知识都是自己一点一点学来的，没有实战项目经验，多亏有前辈收取少量费用就提供项目和讲解，才获得了工作机会，吃水不忘挖井人，博主现在从事开发软件开发、有丰富的编程能力和水平、也学习前辈帮助更多需要帮助的人学习编程！少走弯路！另外可以帮写代码，软件设计！
## 源码获取：
weixin:  a1719101159
