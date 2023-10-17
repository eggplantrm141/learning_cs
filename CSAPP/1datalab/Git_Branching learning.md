# Git_Branching learning

### 一.基础

#### git commit : 提交修改，指向父节点

![image-20231017192136143](C:\Users\yuzhenwei\AppData\Roaming\Typora\typora-user-images\image-20231017192136143.png)

git checkout A：切换到A分支，git branch A :创建A分支

git checkout -b A :创建A分支并切换到A。

git merge A：合并当前分支到A中，若自己为父分支则直接将自己移到A分支。

![image-20231017192456587](C:\Users\yuzhenwei\AppData\Roaming\Typora\typora-user-images\image-20231017192456587.png)

git rebase A,切换到A下方，若自己为父分支则直接将自己移到A分支。

![image-20231017192707005](C:\Users\yuzhenwei\AppData\Roaming\Typora\typora-user-images\image-20231017192707005.png)

### 二、高级

^指向父节点 ^n表示是哪一个父节点，~n，指向第n个父节点

git reset HEAD~1

![image-20231017193208464](C:\Users\yuzhenwei\AppData\Roaming\Typora\typora-user-images\image-20231017193208464.png)

git revert HEAD 

![image-20231017193255112](C:\Users\yuzhenwei\AppData\Roaming\Typora\typora-user-images\image-20231017193255112.png)

### 三、修改提交树

#### 1.git cherry-pick 

git cherry-pick c1 c2 :将当前改动加入C1和C2的改动

![image-20231017193523118](C:\Users\yuzhenwei\AppData\Roaming\Typora\typora-user-images\image-20231017193523118.png)

#### 2.git rebase -i 

图像化修改当前HEAD向上 n的提交，可以删除或者交换顺序

#### 3.git tag v1 c1

给c1标签V1
![image-20231017194140345](C:\Users\yuzhenwei\AppData\Roaming\Typora\typora-user-images\image-20231017194140345.png)

### 四、远程

#### 1.git clone ： 在本地建立与远程一样的树

![image-20231017194451442](C:\Users\yuzhenwei\AppData\Roaming\Typora\typora-user-images\image-20231017194451442.png)

#### 2.git fetch : 从远程下载代码，并更新本地的远程分支与远程的分支一致

![image-20231017194754756](C:\Users\yuzhenwei\AppData\Roaming\Typora\typora-user-images\image-20231017194754756.png)

#### 3.git pull == git fetch + git merge origin/main

![image-20231017194901283](C:\Users\yuzhenwei\AppData\Roaming\Typora\typora-user-images\image-20231017194901283.png)

#### 4.git push : 将自己的本地分支推导远程所追踪的分支，并将本地的远程分支与远程分支同步

![image-20231017195444968](C:\Users\yuzhenwei\AppData\Roaming\Typora\typora-user-images\image-20231017195444968.png)

通过git checkout -b A origin/B  可以将本地的A分支来最终远程的B分支
![image-20231017195638755](C:\Users\yuzhenwei\AppData\Roaming\Typora\typora-user-images\image-20231017195638755.png)

git push A B:将本地的B分支推送到远程的A分支

![image-20231017195821793](C:\Users\yuzhenwei\AppData\Roaming\Typora\typora-user-images\image-20231017195821793.png)

**==这里的git push origin main。指将本地的main分支推送到远程中==**，**==而orgin/main指的是远程的main分支==**