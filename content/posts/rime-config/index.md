---
date: '2026-06-05T20:30:22+08:00'
draft: false
title: 'Rime输入法配置'
summary: '简单记录一下Rime输入法的配置'
categories: '折腾记录'
---
**前言：**  
笔者之前一直在手机上使用离线状态下的百度输入法定制版。直到有一天，笔者发现离线输入状态下，无法使用输入法的 emoji 表情列表，必须要同意百度输入法的用户隐私协议。于是笔者打算重新找一款隐私保护友好，同时功能又比较强大的输入法。经过搜索以及 AI 推荐，最终决定尝试一下开源的 [**Rime**](https://rime.im/)  

*本文所使用的系统环境为 __Windows 10__ 和 __ColorOS 16__*  

---  
### 在 Windows 上使用[**小狼毫输入法**](https://github.com/rime/weasel)  
- 下载安装完成后，在 Windows 系统设置中，找到**语言**选项，在简体中文语言包中添加**小狼毫**键盘，并删除**微软拼音**键盘  
- 配置词库和模型  
  1. 下载[**白霜拼音**](https://github.com/gaboolic/rime-frost)和[**万象语法模型**](https://github.com/amzxyz/RIME-LMDG)  
  解压到小狼毫的用户文件夹（可在右下角状态栏右键输入法图标，点击**用户文件夹**）  
  在小狼毫的**输入法设定**中取消勾选其他输入方案，并勾选**白霜拼音**  
  2. 配置个人使用习惯  
  - 在用户文件夹的 `default.custom.yaml` 中输入以下内容  
  ```yaml
  patch:
    # 1. 使用减号 - 和等号 = 进行“以词定字”
    "key_binder/select_first_character": "minus"
    "key_binder/select_last_character": "equal"

    # 2. 使用方括号 [ 和 ] 翻页
    "key_binder/bindings/+":
      - { when: paging, accept: bracketleft, send: Page_Up }
      - { when: has_menu, accept: bracketright, send: Page_Down }

    # 3. 设置候选词数量为 6 个
    "menu/page_size": 6
  ```
  - 在用户文件夹中找到或新建 `rime_frost.custom.yaml` ，输入以下内容  
  ```yaml
  patch:
    punctuator/half_shape:  # 继承默认标点，并仅修改 / 键为顿号
      __include: default:/punctuator/half_shape
      "/": "、"

    grammar:  #接入万象语法模型
      language: wanxiang-lts-zh-hans
      non_collocation_penalty: -4
      collocation_max_length: 5
      collocation_min_length: 2
      collocation_penalty: -14
    translator/contextual_suggestions: true
    translator/max_homophones: 4
    translator/max_homographs: 2
  ```
  3. 修改完配置文件后，右键小狼毫的图标，选择**重新部署**  
  4. 按 `Ctrl` + `` ` `` 可以打开白霜拼音的**功能菜单**  

### 在 Android 手机上使用[**小企鹅输入法**](https://github.com/fcitx5-android/fcitx5-android)  
- 安装并进行基础设置  
- 下载安装 [**Rime 插件**](https://github.com/fcitx5-android/fcitx5-android/releases)  
在小企鹅输入法的**附加组件**页面中勾选**中州韵**，并在**输入法**页面中添加**中州韵**  
- 配置词库和模型  

  - *以下内容参考自[**万象拼音项目文档**](https://amzxyz.github.io/doc/fcitx5-android/)*  

  1. 下载[**白霜拼音**](https://github.com/gaboolic/rime-frost)和[**万象语法模型**](https://github.com/amzxyz/RIME-LMDG)  
  2. 小企鹅输入法 App → 输入法 → 中州韵设置 → 用户数据目录 → 使用系统文件管理器打开  
  找到以下路径：  
  `/storage/emulated/0/Android/data/org.fcitx.fcitx5.android/files/data/rime/`  
  3. 将下载好的白霜拼音和万象语法模型解压复制到这个文件夹中  
  4. 在该目录中新建一个 `rime_frost.custom.yaml` ，并输入以下内容：  
  或在其他位置创建好该文件，再复制到该目录
  ```yaml
  patch:
    grammar:  #接入万象语法模型
      language: wanxiang-lts-zh-hans
      non_collocation_penalty: -4
      collocation_max_length: 5
      collocation_min_length: 2
      collocation_penalty: -14
    translator/contextual_suggestions: true
    translator/max_homophones: 4
    translator/max_homographs: 2
  ```
  - 注意：**谨慎使用 MT 管理器导入文件！尽量使用手机自带的官方文件管理器！**  
  具体原因请查阅[此文档中的说明](https://amzxyz.github.io/doc/fcitx5-android/)  
  5. 唤出键盘，在设置菜单中点击**重载配置**和**重新部署**  
  该过程需要编译大量文件，**请耐心等待**  
  部署完成后即可选择白霜拼音作为输入方案  

---  
### 关于数据同步  
- 在 Windows 端小狼毫输入法的用户文件夹中，找到 `installation.yaml` 文件  
修改 `installation_id` 这一项为合适的名称，例如 `Windows_PC`  
右键小狼毫的图标，点击**用户资料同步**，会在用户文件夹的 `sync` 目录生成同步文件内容  
- Andorid 端同理，在 `installation.yaml` 中将 `installation_id` 改为合适合适的名称，例如 `Android_Phone`  
唤出键盘，在菜单中点击词库方案（如白霜拼音），然后点击同步按钮，会在 `rime/sync` 目录中生成同步文件内容  
- 同步的具体过程为：  
  1. 将 Windows 端的 `sync` 目录中的**同步数据文件夹**（ `Windows_PC` 文件夹），通过某种方式传输到 Android 端的 `sync` 目录  
  2. Android 端同理，将**同步数据文件夹** `Android_Phone` 传输至 Windows 端的 `sync` 目录
  3. 点击同步按钮，Rime 会将 `sync` 目录中，**来自各个设备的数据文件夹**的 **`*.userdb.txt`** 文件合并至本地数据库中  
      - **`*.userdb.txt`** 就是在不同设备上建立的个人词库，**同步的本质就是交换合并这个文件**  
- 同步工具可选  
  - [**Syncthing**](https://syncthing.net/)  
  - [**Syncthing-Fork**](https://github.com/researchxxl/syncthing-android)  
  - [**FolderSync**](https://foldersync.io/)  
### 注意！  
因为**高版本 Android 系统权限的限制**，在未 Root 的情况下，某些同步工具可能无法正常读写 `Android/data` 目录  
而小企鹅输入法并不支持将 `Android/data` 外的公共目录设定为同步目录，因此请使用支持 **SAF** 框架的同步工具  
或考虑更换[**同文输入法**](https://github.com/osfans/trime)，该输入法会在上级目录 `/storage/emulated/0/` 中创建用户文件夹，可以被同步工具正常读写  
> *因为输入法没有存储权限，所以不支持自定义 sync_dir ，请考虑使用支持 SAF 的目录同步工具，如 [FolderSync](https://foldersync.io/) ，或者使用 root 权限运行 syncthing 以访问输入法的数据目录。*  
> 
> — *[GitHub Issue #452](https://github.com/fcitx5-android/fcitx5-android/issues/452)*  

> *公共文件夹的文件 ，小企鹅只有自己写入的文件的权限，别的软件的创建的文件没有权限读取。只能在默认私有目录，小企鹅才能读写*  
> 
> — [*GitHub Issue #660*](https://github.com/fcitx5-android/fcitx5-android/issues/660#issuecomment-2768906083)  

> *Android App 目前默认没有对非指定目录的访问权限，主要主动申请，但 Fcitx5-android 或者 Rime plugin 没有实现相关功能。所以解决方案是把 `sync_dir` 设置为一个默认就有写权限的位置，比如 `/storage/emulated/0/Documents/RimeSync`。*  
>
> *但注意，公共目录下的文件默认只有创建者有修改的权限。所以你的同步程序（比如 Syncthing ）需要支持主动申请文件权限的功能（一般都支持）。*  
>
> — [*V2EX 讨论帖第 50 楼*](https://www.v2ex.com/t/994246)

~~由于笔者选择Rime的主要原因是隐私保护，而 FolderSync 并不是一款开源软件，Syncthing-Fork 又无法正常进行读写，因此只好放弃自动同步方案，转而使用手动同步方案了（~~