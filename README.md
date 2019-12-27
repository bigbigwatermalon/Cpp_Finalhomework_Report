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
### 总结

