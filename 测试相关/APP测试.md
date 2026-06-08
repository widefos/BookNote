---
title: APP测试
tags:
  - testing
---

APP测试的测试内容


- 功能测试（业务流程测试、功能模块测试）


**专项测试**
- 安装卸载升级
![[Pasted image 20250917164146.png]]
![[Pasted image 20250917164227.png]]
![[Pasted image 20250917164324.png]]

![[Pasted image 20250917164529.png]]
- PUSH消息推送
- 交叉事件测试
	- 交叉事件定义：一个功能正在执行的时候，另外一个事件或者操作对该过程进行干扰的测试
	- ![[Pasted image 20250917165336.png]]
- 用户体验测试
- 兼容性测试


**性能测试**：
- CPU、内存占用
- 启动速度
- 流量、电量消耗
- 流畅度
- 稳定性


**敏捷开发模型**：

互联网软件的软件开发模型

![[Pasted image 20250917162701.png]]



常用ADB命令：![[Pasted image 20250917185136.png]]

卸载/安装软件：
adb install+包名/adb uninstall+包名

清除应用缓存：
adb shell pm clear + 包名


![[Pasted image 20250917185850.png]]


获取日志：adb logcat > 本地文件   这个表示将输出重定向到某个文件当中，从而输出日志


获取APP启动时间：
![[Pasted image 20250917190537.png]]

## 相关笔记
- [[测试相关/测试基础]]
- [[测试相关/功能测试]]
- [[测试相关/接口测试]]




