# vue 项目从零开始3.x
_made by caowujun,2020.11.11_

---
## 1. 安装
命令窗口执行
```bash
npm install vue@next
npm install -D @vue/compiler-sfc  //效果未知
npm install -g @vue/cli
```
检验

```bash
vue --version
```
## 2. 创建
命令窗口执行
```bash
vue create vue3 
```
 
问题1：cnpm安装element-plus 会出错，缺少什么vue@2x,不解，用npm吧 
问题2：element-plus必须第一个装，最后装的时候，会把之前的axios ，i18统统给删掉，不知道为什么
问题3：cnpm install会安装大量垃圾文件夹，3G多

最终执行
```bash
npm install element-plus --save
npm install vue-i18n@next  --save
npm install js-md5 --save 
npm install echarts --save
npm install qs --save
npm install axios --save
npm i --save  @types/js-md5     
```


## 3. vite，待研究
```bash
npm init vite@latest vuevite --template vue
[文档1](https://blog.csdn.net/weixin_43891485/article/details/117635376)
[文档2](https://www.cnblogs.com/wisewrong/p/13717287.html)
```

