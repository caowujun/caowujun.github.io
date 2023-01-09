# github 的 ssh

_made by caowujun,2022.1.19_


现在都是用token，不使用ssh了，后面ssh相关不用看了
---

官方文档：https://docs.github.com/cn/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

## 1. 开启 openssh

在更新存储之前，我们要先检查一下本地的 OpenSSH 服务有没有开启。不然会出错。

开启 ssh 服务的流程为：
1）设置 → 管理可选功能 → 添加功能 → [OpenSSH 服务器]
2）计算机管理 → 服务和应用程序 → 服 务 → OpenSSH Authentication Agent&OpenSSH Server → 右击

作者：思侬
链接：https://juejin.cn/post/6844903831000596488
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## 2. 生成 ssh 代码。

C:\Users\username\.ssh 文件夹下打开 cmd 执行,注意双引号不要丢掉了

```bash
$ ssh-keygen -t ed25519 -C "ssss@163.com"
```

连续回车就行,文件名选择 ssss

然后在 C:\Users\username\.ssh 找到 ssss.pub, 它有一个 sha256，拷贝出来

## 3. ssh 上传代码到 github

用记事本打开 ssss.pub，拷贝密钥到 github/gitlab 的个人 setting 里的 ssh 中。

## 4. 启动服务并追加密钥

```bash
eval "$(ssh-agent -s)"

ssh-add ~/.ssh/id_ed25519
```

如果是在 ssh 目录下，那么运行（在此之前在 windows 机能里追加 ssh，然后在服务打开 open ssh 开头的 2 个）

```bash

ssh-add  id_ed25519
```

就可以用了.

```测试链接
ssh -T git@github.com
```

## 5. 可能问题

有可能有这么个问题，就是每次用，都必须人工重复 3（家里台式机没这个问题，公司台式机也没有，但是笔记本有这个问题）
比较奇葩的问题，换了个目录，就会授权失败.
所以根据 git 官网：https://docs.github.com/cn/github/authenticating-to-github/connecting-to-github-with-ssh/working-with-ssh-key-passphrases

新建一个文件~/.profile
拷贝

```bash
env=~/.ssh/agent.env

agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

agent_start () {
    (umask 077; ssh-agent >| "$env")
    . "$env" >| /dev/null ; }

agent_load_env

# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2=agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add
fi

unset env
```

重启 git bash

手动将 key 文件添加到 ssh agent

```bash
ssh-add ~/.ssh/baaigeini_github
ssh -T git@github.com
```

---

另外一个思路
在 git 安装目录 etc 下面的 bash.bashrc 文件，末尾添加：

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
ssh-add ~/.ssh/id_rsa_tow
```

注意上面 2 个方法不能共存，另外如果用小乌龟，那么在
编辑全局.gitconfig，在末尾添加代码

```json
[credential]
      helper = store
```

## 6. git 仓库初始化

如果项目没有经过初始化，就是指的没有.git 文件夹。
create a new repository on the command line(此时服务器不能有 readme 等文件，必须是个空的)

```bash
git init
touch README.md
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:caowujun/vue3.git
git push -u origin main
```

比如我用 create-react-app 或者 vue create ,创建好的项目本身已经 git init 了，就用下面的。
push an existing repository from the command line

```bash
git remote add origin git@github.com:caowujun/vue3.git
git branch -M main
git push -u origin main
```
