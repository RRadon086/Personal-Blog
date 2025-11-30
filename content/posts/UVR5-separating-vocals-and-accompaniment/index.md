---
date: '2025-11-30T13:39:44+08:00'
draft: false
title: '使用Ultimate Vocal Remover v5提取歌曲的人声和伴奏'
summary: '简单记录一下UVR5的使用'
categories: '音乐制作'
---
Ultimate Vocal Remover可以使用AI技术，将歌曲或音频的人声、伴奏、乐器分离为不同音轨。

官网：[https://ultimatevocalremover.com](https://ultimatevocalremover.com)

下载前请先阅读[**Readme中的说明**](https://github.com/Anjok07/ultimatevocalremovergui?tab=readme-ov-file#installation)，确保自己的电脑系统已经达到了官方的配置要求。


首先前往[**UVR的下载页面**](https://github.com/Anjok07/ultimatevocalremovergui/releases)，按照里面的安装说明选择对应的版本进行下载。由于我是AMD显卡，所以需要下载安装Beta版本的UVR5，这样可以调用DirectML来运行Roformer模型。

***本文所使用的UVR的版本号为v5.6。***


打开软件后，点击左下方的**扳手图标**🔧为软件的设置页面，然后选择上方菜单栏中的**Download Center**切换为模型下载页面。
![模型下载中心](./download_center.webp "模型下载中心")

询问了一下Gemini，得到如下回答，仅供参考。

如果想要最通用、最平衡的人声伴奏分离，可以选择**MDX-Net**架构中的**Roformer Model: BS-Roformer-Viperx-1297**

想要最干净的伴奏可以选择**Roformer Model: MelBand Reformer Kim | Inst V2 by Unwa**

做伴奏，同时保留和声可以选择**Roformer Model: Karaoke MelBand Reformer | (by aufr33 & vipex)**

去混响/去回声可以选择**Roformer Model: BS Reformer Dereverb | (anvview edition)**


如果想要将歌曲细分为不同乐器音轨，可以选择另一个**Demucs**架构。

**Demucs v4: htdemucs_ft**对标准的**4分轨（鼓、贝斯、人声、其他）** 来说效果是最好的。

**Demucs v4: htdemucs_6s**则在标准4分轨的基础上加上了**钢琴轨**和**吉他轨**。


下载好模型后，回到主界面，左侧的**CHOOSE PROCESS METHOD**和**CHOOSE XXX MODEL**可以选择你要使用的模型。
![选择模型](./choose_model.webp "选择模型")
点击上方的**Select Input**按钮选择你要分离人声和伴奏的歌曲文件，**Select Output**按钮设置存放输出文件的目录。其他一些剩余功能，可以根据自己的实际情况进行设置。**将鼠标悬停在设置项的名称上可以查看说明**。
![设置说明](./description.webp "设置说明")
由于我的显卡只有8GB显存，如果使用BS-Roformer-Viperx-1297模型的话，还需要修改一下模型本身的配置文件，否则会爆显存然后软件报错。在左侧选择模型的下拉菜单里，点击**Open Model Folder**打开模型的文件夹，然后打开**model_data\mdx_c_configs**文件夹里的**model_bs_roformer_ep_317_sdr_12.9755.yaml**（可以使用记事本打开），然后将`chunk_size: 352800`这一项改为`chunk_size: 132300`，这样可以降低每次需要存放到显存中进行计算的数据量。

最后点击UVR5主界面下方的**Start Processing**并等待处理完毕，就可以在输出文件目录里看到分离好的歌曲人声和伴奏了。