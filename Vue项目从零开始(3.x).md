###1.安装
```bash
npm install vue@next
npm install -D @vue/compiler-sfc  //效果未知
npm install -g @vue/cli
```
检验

```bash
vue --version
```
###2.创建

vue create vue3 
 
<!-- npm install @vue/composition-api -->
cnpm安装element-plus 会出错，缺少什么vue@2x,不解，用npm吧
npm装element好像会删掉axios
element-plus必须第一个装，最后装的时候，会把之前的axios ，i18统统给删掉，不知道为什么
cnpm install会安装大量垃圾文件夹，3G多

npm install element-plus --save
npm install vue-i18n@next  --save
npm install js-md5 --save 
npm install echarts --save
npm install qs --save
npm install axios --save
npm i --save  @types/js-md5     



###3.vite
npm init vite@latest vuevite --template vue
[文档1](https://blog.csdn.net/weixin_43891485/article/details/117635376)
[文档2](https://www.cnblogs.com/wisewrong/p/13717287.html)

###4.本地项目git
…or create a new repository on the command line
echo "# vue3" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:caowujun/vue3.git
git push -u origin main
…or push an existing repository from the command line
git remote add origin git@github.com:caowujun/vue3.git
git branch -M main
git push -u origin main