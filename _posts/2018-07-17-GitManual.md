---
layout: post
title: "Git权威使用手册"
category: 读书笔记
tags: Notes
---

Git权威指南, 蒋鑫


设置一些Git别名，以便可以使用更为简洁的子命令。


Git还提供了一条git grep命令来更好地搜索工作区的文件内容，


实际上可以用git config命令操作任何其他的INI文件。


·参数--amend是对刚刚的提交进行修补，这样就可以改正前面的提交中错误的用户名和邮件地址，而不会产生另外的新提交。


执行git rm--cached命令时，会直接从暂存区删除文件，工作区则不做出改变。


·使用HEAD代表版本库中最近的一次提交。 ·符号^可以用于指代父提交。


用reflog挽救错误的重置


实际上当reflog中含有该提交的日志过期后，这个提交随时都会从版本库中彻底清除。


文件.gitignore设置的文件忽略是共享式的。之所以称其为“共享式”，是因为.gitignore被添加到版本库后成为了版本库的一部分，当版本库共享给他人（克隆），或者把版本库推送（PUSH）到集中式的服务器（或他人的版本库）时，这个忽略文件就会出现在他人的工作区中，文件忽略在他人的工作区中同样生效。 与“共享式”忽略对应的是“独享式”忽略。独享式忽略就是不会因为版本库共享，或者版本库之间的推送传递给他人的文件忽略。独享式忽略有两种方式： ·一种


·一种是针对具体版本库的“独享式”忽略。


Git提供了一个归档命令：git archive，


可以用于区分编码人和合入人?


参数--pretty=fuller会同时显示作者和提交者，两者可以不同。


·比较里程碑B和里程碑A，用命令：git diff B A

Notes: 1) 比较两个版本的差异?


显示不同版本间该路径下文件的差异。


git diff还可以在Git版本库之外执行，


使用--word-diff参数可以显示逐词比较。


只想查看某几行，使用-L n，m参数，命令如下：


只想查看某几行，使用-L n，m参数，命令如下：


只想查看某几行，使用-L n，m参数，命令如下： $ git blame -L 6，+5 README


二分查找使用自动化测试

Notes: 1) 会很有意思,比如自动化环境自动定位出问题commit


git commit --amend -m


想要将最近的两个提交压缩为一个，


操作过程相当于将该提交导出为补丁文件，然后在当前HEAD上重放，


命令git rebase是对提交执行变基操作，


git rebase --onto C E^ F

Notes: 1) 删掉某些历史commit


提供，对于修改历史提交的提交说明非常方便。


对于修改历史提交的提交说明非常方便。


由里程碑A对应的提交构造出一个根提交至少有两种方法。


其中以.pack结尾的文件是打包文件，以.idx结尾的是索引文件。


对象。松散对象打包后会提高访问效率，而且不同的对象可以通过增量存储节省磁盘空间。


文件反复地修改、反复地向暂存区添加，或者添加到暂存区后不提交甚至直接撤销，就会产生垃圾数据占用磁盘空间。


Git提供了git fsck命令，可以查看到版本库中包含的没有被任何引用关联的松散对象。


用git prune清理之后，会发现： ·用git fsck查看，没有未被关联到的松散对象。


实际操作中会很少用到git prune命令来清理版本库，而是会使用一个更为常用的命令git gc。


执行git pull操作会触发git merge操作，因此也会对本地版本库进行按需整理。


在查看日志时使用参数--decorate可以看到提交对应的里程碑及其他引用。


git cat-file -p mytag2


object 8a9f3d16ce2b4d39b5d694de10311207f289153f type commit tag mytag2 tagger user1 <user1@sun.ossxp.com> Sun Jan 2 14:10:07


git log -1 --pretty=oneline mytag2


如果需要将本地建立的所有里程碑全部推送到远程版本库，可以使用通配符。 $ git push origin refs/tags/*


git push origin refs/tags/*


所谓引用表达式就是用冒号分隔的引用名称或通配符。用在这里代表用远程共享版本库的引用refs/tag/mytag2覆盖本地版本库的同名引用。 $ git pull origin refs/tags/mytag2:refs/tags/mytag2


所谓卖主分支，就是在版本库中创建一个专门和上游代码进行同步的分支，


可以运行git cherry命令查看哪些提交领先（未被推送到上游跟踪分支中）。


基于里程碑v1.0创建发布分支hello-1.x。

Notes: 1) 出规避场景


下面的命令将我对此Bug的修改保存为一个补丁文件。 $ git format-patch jx/v1.1..jx/v1.2

Notes: 1) 不通过commit传递修改


因为是拣选操作，提交时最好重用所拣选提交的提交说明和作者信息，而且也省下了自己写提交说明的麻烦。使用下面的命令完成提交操作。 $ git commit -C hello-1.x^1


因此很多项目在特性分支合并到开发主线的时候，都不推荐使用合并操作，而是使用变基操作。


删除远程共享版本库的user2/i18n分支。 $ git push origin :user2/i18n


那么克隆操作产生的远程分支为什么都有一个名为“origin/”的前缀呢？奥秘就在配置文件.git/config中。


Git提供了将提交批量转换为补丁文件的命令：git format-patch。


如果SSH登录存在问题，可以通过查看服务器端的/var/log/auth.log日志文件进行诊断。


用户可能拥有不只一套公钥/私钥对。为了创建不同的公钥/私钥对，在使用ssh-keygen命令时就需要通过-f参数指定不同的私钥名称。


Gitolite是一款Perl语言开发的Git服务管理工具，通过公钥对用户进行认证，并能够通过配置文件对写操作进行基于分支和路径的精细授权。


Gitosis是Gitolite的鼻祖，同样也是一款基于SSH公钥认证的Git服务管理工具，但是功能要比之前介绍的Gitolite弱一些。


Git最初仅仅是一个可对内容进行追踪、可版本管理的另类的文件系统，


为什么换行符转换要特意强调文本文件呢？这是因为如果对二进制文件（程序或数据）当中出现的换行符进行上述转换，会导致二进制文件被破坏。

Notes: 1) 如何判断文本文件?


关于属性文件，会在后面的章节详细介绍，现在可以将其理解为工作区目录下的.gitattributes文件，


通过配置变量core.autocrlf来开启文本文件换行符转换的功能。


Git可以对文本文件中空白字符的使用是否规范做出检查，


Git的钩子脚本位于版本库的.git/hooks目录下，


稀疏检出就是，本地版本库检出时不检出全部，只将指定的文件从本地版本库中检出到工作区，而其他未指定的文件则不予检出（即使这些文件存在于工作区，其修改也会被忽略）。


如何将新旧两条开发线连接到一起呢？于是就发明了提交嫁接，以实现新旧两条开发线的合并，这样Linux开发者就可以在一个开发分支中由最新的提交追踪到原来位于Bitkeeper中的提交[1]。


