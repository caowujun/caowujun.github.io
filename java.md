# java 学习

_made by caowujun,2022.6.19_

---

## 1. 基础

- System.getProperty("user.dir")//可以获得项目的绝对路径跟地址


## 2.
 @RequestMapping("/test6")
    @ResponseBody
    public User test6(@RequestBody User user){
        System.out.println(user);
        return user;
    }

x-www-form-urlencoded格式代码不需要写contentType,而json格式代码需要加上contentType: ‘application/json;charset=UTF-8’
x-www-form-urlencoded格式传输后端不需要添加@RequestBody注解,而json格式需要
x-www-form-urlencoded格式传输的时候data里的数据不要用’‘包裹,否则会得到null,而json使用data去传输的时候恰恰相反,需要使用’'包裹
————————————————
版权声明：本文为CSDN博主「yikai945」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_23049221/article/details/108621301