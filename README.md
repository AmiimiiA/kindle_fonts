# 替换 iOS Kindle 的默认字体

Rplace the iOS Kindle APP's font.

> 引自：https://bookfere.com/post/959.html

**说明：国区账号登录 APP 时，iOS 设置->通用->语言与地区->地区，必须修改为「中国大陆」，否则会卡在登录界面。**

## 一些字体

- 仓耳今楷预览链接（官网）：http://www.tsanger.cn/category/21
- 03 字重下载链接（官网）：http://www.tsanger.cn/download/仓耳今楷05-W02.ttf
- 04 字重下载链接（官网）：http://www.tsanger.cn/download/仓耳今楷05-W03.ttf

![仓耳今楷](https://bookfere.com/wp-content/uploads/2022/03/kindle-change-font-on-all-platform-1.png)

## 一、iOS 下为 Kindle App 更换字体

由于 iOS 系统限制，无法像 Kindle 设备那样使用自定义字体的方法替换 Kindle APP 下载字体，因此需要配合 iOS 上的网络调试工具重写下载字体的网络地址。

此处以 Surge 为例展示操作方法，配置和信任证书的教程自行网络搜索，其他工具同理。抓包得到 Kindle 下载字体的连接为：

### 国区

- 黑体：
    >https://s3.cn-north-1.amazonaws.com.cn/maes-appexpan-protected-prod/STFontSC/STHeitiSC.ttf?XXXXXXXXXXXXXX
- 楷体：
    >https://s3.cn-north-1.amazonaws.com.cn/maes-appexpan-protected-prod/STFontSC/STKaitiSC.ttf?XXXXXXXXXXXXXX
- 宋体：
    >https://s3.cn-north-1.amazonaws.com.cn/maes-appexpan-protected-prod/STFontSC/STSongSC.ttf?XXXXXXXXXXXXXX
- 圆体：
    >https://s3.cn-north-1.amazonaws.com.cn/maes-appexpan-protected-prod/STFontSC/STYuanMedium-2018-02-16.ttf?XXXXXXXXXXXXXX

### 美区

- 黑体：
    >https://s3.amazonaws.com/maes-appexpan-protected-prod/STFontSC/STHeitiSC.ttf?XXXXXXXXXXXXXX
- 楷体：
    >https://s3.amazonaws.com/maes-appexpan-protected-prod/STFontSC/STKaitiSC.ttf?XXXXXXXXXXXXXX
- 宋体：
    >https://s3.amazonaws.com/maes-appexpan-protected-prod/STFontSC/STSongSC.ttf?XXXXXXXXXXXXXX
- 圆体：
    >https://s3.amazonaws.com/maes-appexpan-protected-prod/STFontSC/STYuanMedium-2018-02-16.ttf?XXXXXXXXXXXXXX

## 二、修改字体信息

**先从第一步获取需要修改字体的下载链接，下载字体，以用来查看字体信息。**

- 以楷体为例，浏览器打开抓包链接下载楷体字体文件，使用在线字体编辑器：

    > FontEditor：https://kekee000.github.io/fonteditor/index.html

**注意事项：①留够内存 ②Edge 一直崩，推荐 Firefox 流览器**

- 如果版本号修改不功，可以使用 [FontForge](http://designwithfontforge.com/zh-CN/Installing_Fontforge.html) 修改

  - 工具栏->Element->Font Info->PS Names->Version
  - 工具栏->Element->Font Info->TTF Names->Version
  - **工具栏->Element->Font Info->Name For Humans 必须为 ASCII，否则无法保存**


### 举个栗子：修改楷体 STKaiti
- 先用 [FontForge](http://designwithfontforge.com/zh-CN/Installing_Fontforge.html) 修改版本号
- FontEditor 打开「霞鹜文楷 GB.ttf」，并在【设置 → 字体信息】中将所有信息替换为 “STKaiti.ttf” 字体中的信息。
- 然后点击 TTF 按钮，导出 TTF 文件
- 上传至自己的 Github 或者其他可以直接访问的地址。


### 修改后的字体

- 黑体 STHeitiSC
    >替换为「仓耳今楷05-W02.ttf」
    https://github.com/yujixia/kindle_fonts/raw/main/STHeitiSC/CangErJinKai05-W02/STHeiti.ttf
- 楷体 STKaiti
    >替换为「霞鹜文楷 GB.TTF」
    https://github.com/yujixia/kindle_fonts/raw/main/STKaiti/LXGWWenKaiGB-Regular/STKaiti.ttf
- 宋体 STSongSC
    >暂缺
- 圆体 STYuanMedium-2018-02-16
    >暂缺


### 三、网络工具的配置

最后得到 302 重写规则，创建如下所示的 Surge 模块

**注意，一定要把代码里的字体地址替换成你上传字体后得到的真实可用的地址**

#### 国区
```bash
#!name=Kindle Fonts Customize CN
#!desc=自定义 Kindel 字体


[URL Rewrite]
# > 黑体 -> 仓耳今楷05-W02
# ^https://s3.cn-north-1.amazonaws.com.cn/maes-appexpan-protected-prod/STFontSC/STHeitiSC.ttf.+ https://github.com/yujixia/kindle_fonts/raw/main/STHeitiSC/CangErJinKai05-W02/STHeiti.ttf 302

# > 楷体 -> 霞鹜文楷 GB
^https://s3.cn-north-1.amazonaws.com.cn/maes-appexpan-protected-prod/STFontSC/STKaitiSC.ttf.+ https://github.com/yujixia/kindle_fonts/raw/main/STKaiti/LXGWWenKaiGB-Regular/STKaiti.ttf 302

# > 宋体 -> 暂缺
# ^https://s3.cn-north-1.amazonaws.com.cn/maes-appexpan-protected-prod/STFontSC/STSongSC.ttf.+

# > 圆体 -> 暂缺
# ^https://s3.cn-north-1.amazonaws.com.cn/maes-appexpan-protected-prod/STFontSC/STYuanMedium-2018-02-16.ttf.+


[MITM]

hostname = %APPEND% s3.cn-north-1.amazonaws.com.cn
```
#### 美区
```bash
#!name=Kindle Fonts Customize US
#!desc=自定义 Kindel 字体


[URL Rewrite]
# > 黑体 -> 仓耳今楷05-W02
# ^https://s3.amazonaws.com/maes-appexpan-protected-prod/STFontSC/STHeitiSC.ttf.+ https://github.com/yujixia/kindle_fonts/raw/main/STHeitiSC/CangErJinKai05-W02/STHeiti.ttf 302

# > 楷体 -> 霞鹜文楷 GB
^https://s3.amazonaws.com/maes-appexpan-protected-prod/STFontSC/STKaitiSC.ttf.+ https://github.com/yujixia/kindle_fonts/raw/main/STKaiti/LXGWWenKaiGB-Regular/STKaiti.ttf 302

# > 宋体 -> 暂缺
# ^https://s3.amazonaws.com/maes-appexpan-protected-prod/STFontSC/STSongSC.ttf.+

# > 圆体 -> 暂缺
# ^https://s3.amazonaws.com/maes-appexpan-protected-prod/STFontSC/STYuanMedium-2018-02-16.ttf.+


[MITM]

hostname = %APPEND% s3.amazonaws.com
```

## 四、替换字体
- Surge 新建本地模块，添加上述模块内容并保存。
- 将 Kindle 下载的字体删除并重启应用。
- Surge 开启字体替换模块。
- Kindle 下载对应的字体。
- 成功后 Surge 关闭字体替换模块。

## 五、特别说明
- 本项目内所有资源文件，禁止任何公众号、自媒体进行任何形式的转载、发布。
- 本项目涉及的数据由使用的个人或组织自行填写，本项目不对数据内容负责，包括但不限于数据的真实性、准确性、合法性。使用本项目所造成的一切后果，与本项目的所有贡献者无关，由使用的个人或组织完全承担。
- 本项目中涉及的第三方硬件、软件等，与本项目没有任何直接或间接的关系。本项目仅对部署和使用过程进行客观描述，不代表支持使用任何第三方硬件、软件。使用任何第三方硬件、软件，所造成的一切后果由使用的个人或组织承担，与本项目无关。
- 本项目中所有内容只供学习和研究使用，不得将本项目中任何内容用于违反国家/地区/组织等的法律法规或相关规定的其他用途。
- 所有基于本项目源代码，进行的任何修改，为其他个人或组织的自发行为，与本项目没有任何直接或间接的关系，所造成的一切后果亦与本项目无关。
- 所有直接或间接使用本项目的个人和组织，应24小时内完成学习和研究，并及时删除本项目中的所有内容。如对本项目的功能有需求，应自行开发相关功能。
- 本项目保留随时对免责声明进行补充或更改的权利，直接或间接使用本项目内容的个人或组织，视为接受本项目的特别声明。
