# Cpp_Finalhomework_Report  
---  
学号：18081635  
姓名：张超  

---  
### 一、通过编译源码方式安装Nebula Graph  
#### 硬件情况  
CPU：酷睿  i7-8750 八核  
编译环境：Ubuntu 16.04 LTS 双系统
#### 编译安装  
在开始使用 **Nebula Graph** 之前，我们通过[编译源码](https://github.com/vesoft-inc/nebula/blob/master/docs/manual-EN/3.build-develop-and-administration/1.build/1.build-source-code.md)方式安装 **Nebula Graph**。根据[视频](https://space.bilibili.com/472621355)学习如何安装 **Nebula Graph**。  
**完全编译大概花费60-80分钟**。  

---  
### 二、修改源代码已实现目标功能
#### 老师的倒数第一个问题：  
[Output the current datetime whenever the command execution outputs the result](https://github.com/vesoft-inc/nebula/issues/1518)  
思路：在每一次循环中，获取当时的时间戳，然后将时间戳格式化输出位标准时间格式。  
#### 遇到的问题  
**1、make编译时出错**  
![image](https://github.com/bigbigwatermalon/learn_git/blob/master/bianyi.png)  
百度无果，然后自己排查。发现它编译的路径是 /home/cc/下载/nebula/build，而它所在的路径是 /home/cc/下载/nebulaa/nebula/build，路径不正确，自然编译失败。这主要是我在刚克隆项目之后把nebula文件夹整体移了位置，创建了一个文件夹来存放它。  
解决方法：移动文件夹到正确目录  
```  
sudo mv ~/下载/nebulaa/nebula ~/下载/nebula  
```  
**2、git commit出错**  
![image](https://github.com/bigbigwatermalon/learn_git/blob/master/commit_error.jpg)  
git commit 的时候报错：  
```  
cpplint code style check failed, please fix and recommit.  
```  
这回遇到的问题同样很刁钻，搜索引擎无果，我也重新安装了cpplint，也把git add的文件cpplint通过了。我新创了一个仓库，但是却能成功git commit。  
因此我猜测，应当是我之前在此目录下学习练习git，创了许多分支，以及一些练习时的神奇操作，导致本地仓库出现问题。因此，我重新clone了一份，重新Fork了项目。
#### 源码修改：  
**源码**  
```  
#include "base/Base.h"
#include <gtest/gtest.h>
#include "time/Duration.h"

using nebula::time::Duration;

TEST(Duration, elapsedInSeconds) {
    for (int i = 0; i < 5; i++) {
        Duration dur;
        auto start = std::chrono::steady_clock::now();
        sleep(2);
        auto diff = std::chrono::steady_clock::now() - start;
        dur.pause();

        ASSERT_EQ(std::chrono::duration_cast<std::chrono::seconds>(diff).count(),
                  dur.elapsedInSec()) << "Inaccuracy in iteration " << i;
    }
}  
```  
**更改之后的代码**           /nebula/blob/master/src/common/time/test/DurationTest.cpp
```  
#include "base/Base.h"
#include <gtest/gtest.h>
#include "time/Duration.h"
#include <time.h>

using nebula::time::Duration;

TEST(Duration, elapsedInSeconds) {
    for (int i = 0; i < 5; i++) {
        Duration dur;
        auto start = std::chrono::steady_clock::now();
        sleep(2);
        auto diff = std::chrono::steady_clock::now() - start;
        dur.pause();
        time_t Nowt = time(0);
        char strTime[50];
        ctime_r(&Nowt, strTime);
        ASSERT_EQ(std::chrono::duration_cast<std::chrono::seconds>(diff).count(),
                  dur.elapsedInSec()) << "Inaccuracy in iteration " << i;
        std::cout << strTime << std::endl;
    }
}  
```  
进入build文件，进行编译，测试。  
```  
cd ~/下载/nebula/build/common/time/test
make
cd _build
./duration_test
```  
![image](https://github.com/bigbigwatermalon/learn_git/blob/master/sendpix5.jpg)  
在这个test中总共有五轮循环，每次循环结束之后，我都输出一个此时的标准格式时间。  

### 三、将本地项目上传到远程仓库 
**在改动源码之前，我已经在nebula目录下创建版本库了。**
```  
git init
```  
**设置用户名和邮箱:**  
```  
git config --global user.name "bigbigwatermalon"
git config --global user.email "1359395518@qq.com"
```  
**添加远程库：**  
```  
git remote add origin https://github.com/bigbigwatermalon/nebula
```  
**管理修改：**  
```
git add DurationTest.cpp   
git status                 //查看暂存区状态
git commit -m "something"  //添加“sonething”的说明
```  
**上传代码:**  
```
git push origin master     //把本地的master分支内容推送到远程新的master分支
```  
#### 我们在github上看到新的master分支与上一版本的分支的改动和区别。  
![image](https://github.com/bigbigwatermalon/learn_git/blob/master/sendpix6.jpg)  
### 心得体会
这学期C++课可谓“一波三折”，这一波三折是针对我学习态度来说的。最开始，我对于C++抱有浓厚的兴趣，头几节课都十分认真听讲。可上了几节课，我发觉，吴老师讲课实在是有点快，加上我们专业课多，平时也没太多时间看书，中间就有一段时间没有听课，前一节课没听，后一节就听不懂了。后来，吴老师上课风格有所改变，慢慢降低了难度，我又开始在C++的大海里遨游。通过本学期课程和作业，我真的学到了很多，学到了许多其他老师永远不会教我们的东西。这次大作业，是我第一次接触开源项目，整体流程下来，我对于开源的过程、形式有了更清晰的认识，学会了许多git的操作。期间也遇到了许多问题，但是在自身努力及请教同学老师之后，那些问题都得到了解决。这回大作业我也尝试发也个PR，但是被毙了。后来我去看别人对于这个问题发的PR，才明白自己之前的想法太简单了，自己还有很长的路要走。不过，至少我也感受了一下大体的流程，真的收获颇丰。非常感谢吴老师，在我看来，吴老师不仅仅是我们的老师，更像是我们的学长，经常会从我们的角度上思考问题，会与我们讲行业上许多实际的问题以及现实的问题。在智科专业读书，我其实是有一些小迷茫的。因为我们学的课程很多很杂，但是都不会深入，许多课也只是浅尝辄止，多而不精。我常常在想，如果就规规矩矩地按照教学计划安排来学习，以后出去了会不会竞争力没有这么大。其次，2019年秋招是算法岗寒冬，许多985毕业生手握顶会都很难找到满意的工作，而四非院校出身的我们压力就更大了，等我们毕业了会不会要求比现在还要高呢，等我们毕业了，是不是AI热潮就过去了呢？我常常在思考这些问题。在C++课上，吴老师也会和我们聊这些。吴老师的话以及吴老师的经历启发了我。印象最深的就是这句话：人生就是一个大坑，你永远不知道以后会发生什么。犹记得吴老师课上和我们说，在他上学的时候，神经网络是个大坑，谁去做，谁没饭吃。可谁想，现在他们都发了大财。虽然吴老师没有赶上AI这波浪潮，但是吴老师凭借自己过硬的水平，还是成为了一名纳税几十万的光荣纳税人。从吴老师话语和人生经验中，我有所感悟，心中密布的乌云彻底消散：我们永远不知道明天会发生什么，我们能做的就是做好今天的事情，今天的我们就是好好学习，掌握一些技术，做一些研究。
