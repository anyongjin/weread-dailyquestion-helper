# 微信读书每日一答辅助

## 功能描述
简单的说就是每隔一段时间截张图，OCR得到文字询问大模型。

## 配置环境
- Python 3.10.4
- 微信Macos版  
- 百度AI平台
- 百度智能云开通文心一言（可选  推荐）正确率70%~80%
- 智谱AI平台账号（可选）正确率60%

## 原理
1. 电脑端对小程序截图
2. OCR识别问题和答案
3. 根据问题去百度搜答案
   
    > 选择答案的依据是，以问题作为关键字进行搜索，在搜索结果的首页中，选择出现频率最高的答案作为候选结果，并把它的上下文作为依据。
4. 显示AI回答到控制台，用户点击作答
5. 循环执行1，2，3，4

## 使用方法

**注**：
> 因为Macos没办法获取到小程序的窗口，所以需要手动测量小程序窗口大小，调整题目和答案位置。  
> 文件位置：`process/ScreenCapture.py`
> 截图效果会输出在`output/images`目录下，可参考校对位置


1. 登陆[百度AI开放平台](https://console.bce.baidu.com/ai/#/ai/ocr/app/list)，创建一个通用场景OCR应用(每天免费1w次够用了)，并在应用列表中查看AppID，API Key和Secret Key
2. 打开百度网站登录，复制出cookie；浏览器打开开发者工具，从网络请求中找到cookie复制，注意双引号前加斜线
2. 在根目录下仿照configdemo.json创建一个config.json文件，把上面得到的值填进去
3. 用**电脑**打开微信读书小程序，并**保持在最前**，且不要被遮挡，放在屏幕左上角，严格对齐！
4. 运行根目录下的main.py
5. 进入每日一答页面开始答题，隔一段时间控制台就会输出结果：

注意：  
百度智能云开通千帆大模型平台后，创建一个应用，然后在计费管理中给ERNIE开通按量计费，然后充值个三五块钱，否则默认只能问四五个问题大概。
---

仅供学习参考，最近出题越来越难了，文心一言正确率70~80%，智谱60%左右，讯飞星火测试正确率不高故未接入。
此项目本来使用Click来自动点击，因正确率不够高，没有必要，所以注释掉了。  
目前问题和回答区域一起截图OCR识别，速度会快一些。  
自动化答题总时间 = 截图时间 + OCR时间 + 大模型作答时间  

> 目前已知问题：
> 1. 有时候图片判断不同，然后重复OCR作答。
> 2. 大模型有时候对同一个问题两次回答不一样
> 3. 大模型回答准确率不够高

问答输出示例：
```text
发现新图片，OCR识别文字...
问题: 标志着日本从奴隶社会向封建社

会社会过渡的事件是？

幕府统治的建立

大化改新

-----------------
AI回答: 标志着日本从奴隶社会向封建社会社会过渡的事件是 大化改新。【正确】
-----------------

发现新图片，OCR识别文字...
问题: 小说  《茶花女》的作者是谁？

大仲马

小仲马

-----------------
AI回答: 正确答案是“小仲马”。【正确】
-----------------

发现新图片，OCR识别文字...
问题: 对亚洲的地理位置，  说法正确的

是？

全部都在东半球

亚州西临地中海

-----------------
AI回答: 对亚洲的地理位置，说法正确的是：全部都在东半球。【错误，西临地中海】

亚洲全部位于东半球，是世界上最大的大洲之一。它的东、南、北三面分别濒临太平洋、印度洋、北冰洋。西与欧洲相连，分界线是乌拉尔山脉、里海、大高加索山脉、土耳其海峡。
-----------------

发现新图片，OCR识别文字...
问题: “席梦思”三个字来源于什么？

一个人名

种材质

-----------------
AI回答: 正确选项是“一个人名”。【正确】

“席梦思”最初是指一位名叫“Zalmon G. Simmons”的美国发明家于1870年发明的弹簧床垫。后来，“席梦思”逐渐成为床垫的通用名称，但并不是所有床垫都可以被称为“席梦思”。
-----------------

发现新图片，OCR识别文字...
发现新图片，OCR识别文字...
问题: 在无脊雄动物中，  星虫的活动和

分布范围最大，  主要原因是？

体表有外骨骼

般有两对翅

-----------------
AI回答: 在无脊雄动物中，星虫的活动和分布范围最大，主要原因是体表有外骨骼。

正确答案是：体表有外骨骼。【正确】
-----------------

发现新图片，OCR识别文字...
问题: 当你注视同学眼睛时，  看到对方

眼球的白色部分称为？

玻璃体

巩膜

-----------------
AI回答: 正确答案是“巩膜”。【正确】
-----------------

发现新图片，OCR识别文字...
问题: 下面哪一个是电视剧《笑傲江

湖》  中梅庄守门人丁坚的绰号？

一
字电创

九曲剑

-----------------
AI回答: 电视剧《笑傲江湖》中梅庄守门人丁坚的绰号是“一字电剑”。【正确】
-----------------

发现新图片，OCR识别文字...
问题: “南北对话”中的“北”指的是？

发展中国家

发达国家

-----------------
AI回答: “南北对话”中的“北”指的是发达国家。【正确】
发展中国家与发达国家之间的对话，旨在促进发展中国家的发展和改善其经济状况，并加强发展中国家的经济合作和贸易关系。发展中国家包括中国、印度、巴西、南非等国家，这些国家在经济 
发展方面面临着许多挑战，如资金不足、技术落后、基础设施薄弱等。
发达国家则包括美国、欧洲联盟等国家，这些国家在经济发展方面已经取得了显著进展，具有较为先进的经济技术和较高的生活水平。
因此，“南北对话”中的“北”指的是发达国家，而发展中国家则指的是那些在经济发展方面面临挑战的国家。
-----------------

发现新图片，OCR识别文字...
发现新图片，OCR识别文字...
发现新图片，OCR识别文字...
问题: “鸳鸯”在古代最早的含义是？

A.姐妹

B.兄弟

-----------------
AI回答: B.兄弟【正确】
-----------------

发现新图片，OCR识别文字...
发现新图片，OCR识别文字...
发现新图片，OCR识别文字...
问题: GB[255,  0,  255]  值的色彩

是什么？

浅灰色

墨绿色

-----------------
AI回答: 这个问题的答案应该是“浅灰色”。【错误，灰色是多个元色混合而成，故必定不是灰色】
-----------------


发现新图片，OCR识别文字...
问题: 《侠客行》中谁承诺“如果有人

得到玄铁令就满足他三个条

件”？

丁不三

谢烟客

-----------------
AI回答: 正确答案是“丁不三”。【错误】
-----------------

发现新图片，OCR识别文字...
问题: 《侠客行》  中谁承诺“如果有人

得到玄铁令就满足他三个条

件”？

丁不三

谢烟客

-----------------
AI回答: 正确答案是“谢烟客”。【正确】
-----------------
```



## 致谢
[@maotoumao](https://github.com/maotoumao)