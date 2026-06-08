---
title: WebDriver作用
tags:
  - testing/selenium
---

**1. 你（测试脚本）**：你是一个“指挥官”，用代码写下指令。  
* 指令示例（Python）：  
`python driver.get("https://www.example.com") # “去 example.com 网站！” driver.find_element(By.ID, "search").send_keys("手机") # “找到搜索框，输入‘手机’！” driver.find_element(By.ID, "submit").click() # “找到提交按钮，点击它！”`

**2. WebDriver**：它坐在你和浏览器之间，像一个“翻译官”兼“监工”。  
* **它听懂你的指令**（通过你写的代码）。  
* **它把指令翻译成浏览器能理解的语言**（通过 **浏览器驱动**）。  
* **它把浏览器的反应（成功/失败/找到的元素）汇报给你**。

**3. 浏览器驱动**：比如 ChromeDriver、GeckoDriver（用于Firefox）。这是每个浏览器厂商提供的“**专用遥控器**”。  
* WebDriver 拿着“Chrome遥控器”就可以控制Chrome浏览器。  
* 拿着“Firefox遥控器”就可以控制Firefox浏览器。

**4. 浏览器**：如 Chrome, Firefox, Edge。这是最终被操作的”**实体**”。它接收来自”遥控器”的命令并执行：打开窗口、点击链接、填写表单等。

## 相关笔记
- [[测试相关/Selenium/操作行为]]
- [[测试相关/Selenium/元素定位]]
- [[测试相关/Selenium/等待机制]]

