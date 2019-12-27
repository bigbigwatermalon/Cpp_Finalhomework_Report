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
#### 遇到的问题：  
**1、make编译时出错**  
#### 源码修改：

