---
date: '2026-11-14T04:40:06+08:00'
draft: false
title: '如何让 AI 帮忙看 Spark 的性能检测报告'
summary: '第一次学习使用 Agent，制作了一个小工具'
categories: '折腾记录'
---
[**Spark**](https://www.mcmod.cn/class/4073.html) 是用于 **Minecraft** 客户端、服务端的性能分析插件/模组，可以帮助寻找游戏卡顿的原因。但生成检测结果后，官方网页查看器里的数据内容太过于专业，对小白来说非常不友好。  
现在有了非常方便的 AI，因此可以借助 AI 来分析 Spark 的检测报告，给出简单易懂的解读。  
笔者 **Vibe Coding** 了一个工具，可以更方便地让 AI 阅读并分析 Spark 的检测结果报告。

---
## 工具的制作背景
需要将 Spark 生成的检测报告上传给 AI，才能进行分析解读。Spark 的网页查看器里有一个按钮，可以将检测报告导出为 `.sparkprofile` 格式的本地文件，但这是 Spark 自己的专有格式，AI 无法直接读取。  
作者[在官方文档里提供了一种方法](https://spark.lucko.me/docs/misc/Raw-spark-data)，**可以将网页查看器里的检测报告直接转换为 JSON 格式**，JSON 文件是可以直接被读取的。只需要在检测报告的网页链接后面加上 `?raw=1&full=true` 这个参数。  

不过**这个方法存在局限性：** 
> By default, these endpoints only return the metadata associated with the session. This should be sufficient for most use-cases.
>
> If you really-really want the actual sampler/heapdump data too, firstly, please consider downloading/parsing the raw data yourself. It saves resources at my end! (see below for info)
>
> If this isn't feasible, you can also append &full=true to the URL and get everything back, but be warned, this can potentially return tens of megabytes of JSON!  
>
> —— [Spark 官方文档 **Raw spark data** 章节](https://spark.lucko.me/docs/misc/Raw-spark-data)  

- `full=true` 参数会让得到的 JSON 文件体积变得非常巨大，直接让 AI 读取很容易占满上下文窗口
- 只使用 `raw=1` 参数得到的只有一些**元数据（metadata）**，缺少最关键的**游戏线程占用情况**
- 同时作者提倡尽量不要使用 `full=true` 参数，这可以节省他那边的服务器资源，作者建议下载检测报告的数据在本地进行解析
	- 但使用作者提供的[原始版 `spark2json` 工具](https://github.com/lucko/spark2json)进行本地转换，需要先安装 Docker 或者 Node.js 等运行环境，配置起来有些麻烦  

笔者查阅资料时发现，**社区里已经有很多现成的项目**，同样可以将 `.sparkprofile` 文件转换为 `.json` 文件。最重要的是，可以对数据进行精简摘要，**降低 JSON 文件的体积**，方便上传给 AI 进行分析解读。  
**但它们也存在各自的局限性**，比如只有前端网页的版本，没有可以本地使用的发行版；或者本地使用同样需要先配置运行环境。  

因此笔者让 **AI** 参考复用了这些已有的社区项目，**制作了一个免安装、免配置的本地转换工具**。
- 无需安装前置运行库，开箱即用
- 在本地浏览器中完成文件转换
- 输出一个经过**精简摘要**后的 JSON 文件，体积大小适合 AI 读取分析  
---
## 使用该工具前的准备工作
1. 在游戏中运行 `/spark` 相关指令，得到检测结果的网页链接
	- **Spark 模组本身的用法**请查看**社区相关教程**或 [**Spark 的官方文档**](https://spark.lucko.me/docs)
2. 点击链接打开浏览器，跳转到 Spark 的网页查看器页面
3. 点击网页查看器右上角的**导出**按钮，如图所示
{{< figure
  src="./export_button.webp"
  align="left"
  alt="Spark 网页查看器导出按钮">}}
  将 Spark 检测报告的原始数据导出为 `.sparkprofile` 文件并下载

---
## 工具介绍
**工具项目的 GitHub 仓库**：[https://github.com/RRadon086/spark-report-converter](https://github.com/RRadon086/spark-report-converter)  
开箱即用的完成品请在仓库的 [**Releases 页面**](https://github.com/RRadon086/spark-report-converter/releases)下载，

**工具的使用方法**见 GitHub 仓库里的 [**`app/USAGE.md`**](https://github.com/RRadon086/spark-report-converter/blob/main/app/USAGE.md)，或下载解压后文件夹里的 **`使用说明.pdf`**  

### 注意事项：
- **本工具的代码完全由 AI 生成**。  
	虽然已进行 AI 自动化测试和真实样本验证，但测试环境与样本范围有限，仍可能存在尚未发现或未被现有测试覆盖的问题。  
	如果你需要经过更广泛验证的方案，可以选择其他的社区工具。  
- 仅对 **Forge** / **NeoForge** 平台进行了测试，其他模组加载器和插件服务端**未进行测试**，实际效果未知
- 仅支持 Spark 性能采样器的 **`.sparkprofile`** 文件，不支持 heap、health、Spark 网页链接或批量转换
- 如果可以的话，尽量使用多个不同的 AI 进行分析解读，**一个 AI 找问题可能不够全面**  
- AI 结论并不是百分百正确，执行指令、修改配置或文件前，**一定要先进行备份！**

### 提示词：  
以下是笔者让 AI 生成的一份提示词模板，**仅供参考**，也可以在此基础上修改为适合自己的版本。  
```text
我是一个 Minecraft 小白玩家。这是我用 Spark 模组生成的性能检测报告，请完整阅读后用通俗易懂、容易理解的中文语言分析。如果无法完整读取，请直接告诉我。  
我的游戏环境是：（单人游戏 / 联机服务器）  
实际问题是：（具体怎么卡，卡顿是什么样子的，什么场景下会触发卡顿等等）  
请结合我描述的现象，直接找出报告中最可能导致问题的因素，并按可能性排序，不要只是罗列所有异常。请明确说出你认为最可能的原因及其依据；不确定的地方可以说明，但不要因此回避给出判断。  
请先根据报告作出初步判断，再使用网络搜索查找相关的社区讨论、模组已知问题或修复方法。附上来源，并注意是否适用于我的游戏和模组版本；无法联网或没有可靠资料时请如实说明。  
解决办法请优先给出简单、安全、可逆的操作。涉及指令、删除实体、修改配置或文件、调整模组时，请说明风险、影响范围和需要备份的内容，不要只给命令让我直接执行。  
```
使用时请将里面的**游戏环境**和**实际问题**按照自己的情况填写好，尤其是**实际问题**，尽量描述得具体详细一点

---
## 参考资料与鸣谢  
详细的**参考项目说明**请查阅 GitHub 仓库里的 [**`app/NOTICE.md`**](https://github.com/RRadon086/spark-report-converter/blob/main/app/NOTICE.md)，或下载解压后文件夹里的 **`来源与许可证说明.pdf`**  
- [Spark 官方文档](https://spark.lucko.me/docs)  
	- 如果需要 Spark 模组本身的使用说明，请优先查阅它  
- [FastMirror-MC/spark-viewer-extract](https://github.com/FastMirror-MC/spark-viewer-extract)  
	- 精简摘要、缩减体积的实现复用了此项目的源代码，非常非常感谢该作者
	- 因为该项目输出的检测报告有点过于精简了，缺少一些硬件配置、实体、区块等信息  
	于是笔者便让 AI 把 Spark 原始数据中的完整 **metadata** 信息合并了进来  
		- 也就是前文所述的**只使用 `?raw=1` 参数得到的 JSON 文件**
		- 缺点是文件体积可能会稍微增大一点点