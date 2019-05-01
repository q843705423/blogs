git使用的场景模拟


开发一个大型项目
master分支:作为正常的项目分支
dev分支:作为正常的开发分支
dev-Zhangsan分支:作为Zhangsan的开发的分支
dev-Lisi分支:作为Lisi的开发的分支

假设原本有2个类已经开发完毕
User
Goods


然后项目要增加两个新功能，交给Zhangsan和Lisi去开发
Zhangsan新增了一个Boast类
Lisi增加了一个Fason类

先来列举一下git分支操作的基本命令

|命令|用处|
|-----------|------------------|
|git branch dev|创建dev分支|
|git branch|列举所有分支|

