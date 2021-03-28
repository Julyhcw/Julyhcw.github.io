title: Ubuntu上将本地项目上传至Github
categories: git
tags: 
   - git
---
## 一些准备  
本文将会详细介绍如何将本地的项目文件上传到远程的github仓库进行托管,本次操作基于以下环境:  
- Ubuntu 18.04LTS 64bit
- git version 2.17.1
> 一般都是通用的哈,不必担心版本的问题,win10环境下操作也是一样的,就是要下载git的客户端.  

<!-- more -->
## 注册账号及创建仓库  
首先去[官网](https://github.com/)注册一个账号,注册完之后依次点击右上角的图像 >> your profile >> Repositories >> New,然后按如下图点击创建仓库:
![](http://pbcgmnu5b.bkt.clouddn.com/00.png)
具体配置根据情况来.


## 设置SSH key  
本地与远程github之间建立连接需要通过ssh加密进行,因此需要设置ssh key,首先查看本地是否存在ssh key,在终端输入:
  
![](http://pbcgmnu5b.bkt.clouddn.com/2018-10-07%2011-26-11%20%E7%9A%84%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)
  
如果不存在id_rsa和id_rsa.pub文件说明还没创建ssh key,可在终端输入如下:
  
![](http://pbcgmnu5b.bkt.clouddn.com/git_00.png)  

其中''里面的邮箱是你创建github账号时设置的邮箱,在这过程中会让你设置密码,不想麻烦直接回车选择空密码也行,有上面的显示说明创建成功,进入 　<font color=#DC143C >cd .ssh/</font>,其中　<font color=#DC143C >id_rsa</font>　和　<font color=#DC143C >id_rsa.pub</font>　是一组密钥对,前者是私钥,后者是公钥.  
  
接下来将公钥的内容添加到github中,具体为:依次点击右上角头像,Setting >> Personal settings >> SSH and GPG keys.在SSH keys标签的右下方点击 NEW SSH Keys会弹出两个文本框,其中的Title可以随意写,最好有点标志性,防止忘记,然后将　<font color=#DC143C >id_rsa.pub</font>　的内容复制到另一个文本框中,点击　<font color=#DC143C >Add SSH Key</font>　就可以啦.  
> 另外复制公钥的内容的时候最好使用  
<font color=#DC143C >cat id_rsa.pub</font>　  
命令读取出来再复制,如果直接打开使用其它文件打开复制很可能会报错,会额外复制其它的内容,特别在win10环境下用记事本这类软件.

## 项目上传  
一般来说git上传项目可分为四个部分:  
- 本地待上传文件  
- 缓存区  
- 本地仓库  
- 服务器仓库

这样划分方便理解接下来的上传命令,就我经验,上传个两三次就可以理解啦,中间可能会有报错,但是一般Google都能找到解决方案,接下来就完整复现一次上传的流程.  
首先确认git是否安装,如果没有可使用:

sudo apt install git

然后进入要上传的项目的文件夹,比如我这边是<font color=#DC143C >hcw_github</font>文件夹,具体命令如下:


```python
cd hcw_github/　
git init  #该命令是在你的项目文件下初始化一个Repository,执行成功后会生成一个.git的隐藏文件
ls -a #查看隐藏文件
git add ./ #该命令是将当前目录下的所有文件加入到本地暂存区,执行成功后不会有任何提示
           #如果只想上传某一个文件就添加该文件的具体名称即可
git status #该命令将你本地工作区和暂存区的版本进行比较,查看当前的状态.
git commit -m '这里写注释的也就是添加文件的描述' #该命令会将暂存区的文件提交到本地的历史区,只有在本地历史区的内容才可以提交到github.
                           #另外执行该命令后所有待上传的文件都只是在本地,还和github没有关系.
                           
    
```

上面命令的结果如下图所示:![](http://pbcgmnu5b.bkt.clouddn.com/2018-10-07%2011-16-51%20%E7%9A%84%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)


接下来开始与github进行交流,命令如下:


```python
git remote add origin '这里填写你的github上的仓库链接' 
```

仓库链接就是下图绿色字体旁边显示的那串链接:![](http://pbcgmnu5b.bkt.clouddn.com/01.png)


```python
git pull origin master　#该命令是先将ｇithub上的文件拉下来，一般在每次提交前都要进行ｐull，防止冲突．
　　　　　　              #如果报错：fatal: 拒绝合并无关的历史，可以使用如下命令：
git pull origin master --allow-unrelated-histories
    　　                 #执行后可能会跳出一个打开的文档，可以根据文档进行保存退出就可以了．
```

执行上述命令后会在你待上传的目录下多了一个＇README.md＇的文件，如果你的远程github上有的话．


```python
git push -u origin master #这一步就是向github提交你的项目．
```

最后的结果如下图所示：![](http://pbcgmnu5b.bkt.clouddn.com/2018-10-07%2011-18-11%20%E7%9A%84%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)

最后在github上就可以看到上传的项目啦．  
另外本次参考的一些文档如下：　　
- [Ubuntu环境如何上传项目到GitHub网站](https://blog.csdn.net/ajianyingxiaoqinghan/article/details/70544159)  
- [当 git pull 碰到拒绝合并无关历史](https://blog.csdn.net/lddtime/article/details/77817317)
