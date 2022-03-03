官方文档：https://docs.github.com/cn/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
在更新存储之前，我们要先检查一下本地的 OpenSSH 服务有没有开启。不然会出错。
开启 ssh 服务的流程为：

设置 → 管理可选功能 → 添加功能 → [OpenSSH服务器]
计算机管理 → 服务和应用程序 → 服 务→ OpenSSH Authentication Agent&OpenSSH Server → 右击

作者：思侬
链接：https://juejin.cn/post/6844903831000596488
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
###1.生成ssh代码。
C:\Users\username\.ssh文件夹下打开cmd执行,注意双引号不要丢掉了
```bash
$ ssh-keygen -t ed25519 -C "baaigeini@163.com"
```
连续回车就行,文件名选择baaigeini
然后在C:\Users\username\.ssh 找到baaigeini.pub, 它有一个sha256，SHA256:m8Kt+jqK5dk01pSzfQdDnqrpPtCvZHNZ3nZFa0/W8aE baaigeini@163.com

###2.ssh上传代码到github
用记事本打开baaigeini.pub，拷贝密钥到github/gitlab的个人setting里的ssh中。

###3.启动服务并追加密钥，
```bash
eval "$(ssh-agent -s)"    

ssh-add ~/.ssh/id_ed25519 
```
如果是在ssh目录下，那么运行（在此之前在windows机能里追加ssh，然后在服务打开open ssh 开头的2个）
```bash  

ssh-add  id_ed25519 
```
就可以用了.
```测试链接
ssh -T git@github.com
```

###4.可能问题
有可能有这么个问题，就是每次用，都必须人工重复3（家里台式机没这个问题，公司台式机也没有，但是笔记本有这个问题）
比较奇葩的问题，换了个目录，就会授权失败.
所以根据git官网：https://docs.github.com/cn/github/authenticating-to-github/connecting-to-github-with-ssh/working-with-ssh-key-passphrases

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
重启git bash


手动将key文件添加到ssh agent
```bash
ssh-add ~/.ssh/baaigeini_github
ssh -T git@github.com
```

另外一个思路
在git安装目录etc下面的bash.bashrc 文件，末尾添加：
```bash
eval "$(ssh-agent -s)"    
ssh-add ~/.ssh/id_rsa    
ssh-add ~/.ssh/id_rsa_tow
```

注意上面2个方法不能共存，另外如果用小乌龟，那么在
编辑全局.gitconfig，在末尾添加代码

[credential]
      helper = store