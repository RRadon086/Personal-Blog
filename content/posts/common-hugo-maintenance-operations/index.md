---
date: '2026-06-01T16:15:21+08:00'
draft: false
title: '常用博客维护操作备忘'
summary: '我早该做这件事的'
categories: '博客维护'
---
**前言：** 因为笔者没有养成良好的博客写作习惯，经常隔很长时间才会写一篇文章，每次都需要复习各种常用的基础操作，而现查资料有些浪费时间，所以打算专门写一篇文章来记录，方便自己以后查阅。  

*本文并不是全面的博客维护指南，仅记录笔者日常的使用习惯，不保证适用于所有人，未来可能会有所修改。*  

---  
## 博客框架  
- [**Hugo**](https://gohugo.io/documentation/)  
  - 安装方式：下载 [**GitHub Release**](https://github.com/gohugoio/hugo/releases) 中的文件直接安装  
  - 更新：直接使用新版本文件进行覆盖  
  **修改hugo版本后，需在Cloudflare Pages的环境变量中指定新的Hugo版本**
  - Windows 终端：[**PowerShell 7**](https://gohugo.io/getting-started/quick-start/#commands)  
- [**PaperMod**](https://github.com/adityatelange/hugo-PaperMod/wiki)  
  - 安装方式：[**Git Submodule**](https://github.com/adityatelange/hugo-PaperMod/wiki/Installation#installingupdating-papermod)  
  - 更新  
  ```cmd
  git submodule update --remote --merge
  ```  
- **忽略 public 目录**  
  部署至 Cloudflare Pages 不需要包含 **public** 目录，Cloudflare 会自动构建发布  
  因此需要进行以下操作：  
  - 将 **public** 目录从 Git 追踪中移除（仅取消追踪，不删除本地文件）  
  ```cmd
  git rm -rf --cached public
  ```  
  - 将 **public** 目录加入 **.gitignore** 黑名单  
    - 在站点根目录下，找到 **.gitignore** 文件或新建该文件  
    - 用记事本在文件最末尾新起一行，加上 `public/` 并保存  
    - 使用 `git add` 命令将 .gitignore 文件加入**缓存区**  
    ```cmd
    git add .gitignore
    ```  
    - 检查最终结果  
    ```cmd
    git status
    ```  
    返回的列表内不应该再有 **public** 目录和其他**未加入缓存区的文件**  

---
## 文章写作  
- **在终端中切换至博客站点路径**  
- **新建一篇文章（页面捆绑包形式）**  
```cmd
hugo new content content/posts/my-new-post/index.md
```  
- [**页面捆绑包**](https://gohugo.io/content-management/page-bundles/)  
  可以将**页面内容**（Markdown 文件）与**相关资源**（图片、PDF、数据文件等）打包放在同一个文件夹里，方便进行管理  
  - **叶子捆绑包**：相当于一片树叶，处于分支的末端，代表一个**独立的页面**，文件夹内包含一个 `index.md` 文件，且该文件夹**不能**包含子页面。文件夹内的所有资源都专属于这篇文章  
  常用来在 `index.md` 里写具体的博客文章，在文件夹内存放图片插入到文章中  
  - **分支捆绑包**：相当于一根树枝，代表一个**列表页面**，包含一个 `_index.md` 文件，下面可以嵌套包含更多的子页面或其他捆绑包  
  首页、分类页、文章归档列表等都属于分支捆绑包  
- **启动 Hugo 本地服务器测试网页**  
```cmd
hugo server -D
```  
`-D` 代表包含被标记为**草稿**的文章  
- **[`figure`](https://gohugo.io/content-management/shortcodes/) 短代码插入图片**  
  - 可以非常方便地对插入的图片进行控制  
  例如添加图片说明、点击链接、控制尺寸和对齐方式  
  - 图片格式应使用 `.webp`  
  - [转义](https://discourse.gohugo.io/t/how-to-display-the-source-code-of-a-shortcode-in-a-code-block/48717)  
  在短代码内部添加 `/*` 和 `*/`  
  例如 `{{</*/* ... */*/>}}`
  - 部分参数：  
    - **`src`**  
    图片的路径或链接  
    页面捆绑包可以使用相对路径 `./img.webp`  
    - **`width`**/**`height`**  
    图片的尺寸，直接填写整数  
    只填写一项时，另一项会自动按比例缩放  
    - **`align`**  
    [**PaperMod** 主题提供的图片对齐参数](https://github.com/adityatelange/hugo-PaperMod/wiki/FAQs#centering-image-in-markdown)  
    居中对齐为 `align="center"`  
    - **`title`**  
    图片的标题  
    通常显示在说明文字上方  
    - **`caption`**  
    图片底部的说明文字  
    **支持 Markdown 语法**  
    - **`link`**  
    点击图片后跳转的网址  
    - **`alt`**  
    图片的替代文本  
    在图片无法加载时显示  
```text
{{</* figure
  src="./img.webp"
  width="400"
  align="center"
  title="示例图片"
  caption="这是一张用来**演示**的图片"
  link="https://rradon.pages.dev"
  alt="用于演示的图片"
*/>}}
```

---  
## Markdown  
*[菜鸟教程](https://www.runoob.com/markdown/md-tutorial.html)*  
- **标题**  
  - 使用 `#` 标记，`#` 与文字之间必须有 1 个空格  
  - `#` 数量对应标题等级，例如 3 个 `#` 代表三级标题  
  - 正文直接使用**二级标题**，因为博客文章最顶部的主标题相当于一级标题  
  ```markdown  
  ## AAA  
  ### BBB  
  ```  
- **段落换行**  
  - 使用 **2 个以上的空格 + 回车**  
  - 或者直接在两个段落之间**添加 1 个空行**  
- **分割线**  
  - 使用 **3 个连字符** `---`  
  - 分割线所处的段落与上文段落之间必须有**空行**，否则会加粗上个段落的文字  
  ```markdown  
  AAA  

  ---  
  BBB  
  ```  
- **字体**  
  - **粗体**：使用 2 个星号 `**` 包围文字  
  - **斜体**：使用 1 个星号 `*` 包围文字  
  - **粗斜体**：使用 1 个星号 `***` 包围文字  
  - **使用星号** `*` 而不是下划线 `_`  
  星号在各种 Markdown 解析器中兼容性更好  
  - **中/英文/数字混合**时，在中/英文/数字前后加空格以提高可读性 
  - 强调符号**内部有标点符号**，同时前/后方又有其他文字，**需要在强调符号前/后加 1 个空格**  
  不加空格时 Hugo 的 Goldmark 解析器无法将其视作正确的 Markdown 语法，**会渲染出星号本身**  
  ```markdown  
  AAA **[BBB](CCC)**
  **DDD:** EEE
  ```  
- **删除线**  
  - 在文字的两端加上两个波浪线 `~~`  
  `~~AAA~~`
- **链接**  
  - 语法：`[链接文字](链接地址 "可选的标题")`  
- **列表**  
  - **无序列表**使用**减号** `-` 作为标记  
  与文字之间必须有 1 个空格  
  ```markdown
  - AAA
  - BBB
  - CCC
  ```  
  - **有序列表**使用**数字**并加上 `.` 号来表示  
  与文字之间必须有 1 个空格  
  可用来展示**有顺序要求的步骤或项目**  
  ```markdown
  1. AAA
  2. BBB
  3. CCC
  ```  
  - **列表嵌套**  
    - 子列表需要缩进 2-4 个空格（**推荐 2 个**）再使用 `-` 进行标记  
    - 保持一致的缩进长度  
    - 可以无限层嵌套，但实际使用中建议**不超过 3 层**  
  ```markdown
  1. AAA  
    1. aaa  
    2. bbb  
  2. BBB  
      - aaa  
      - bbb
  ```  
- **转义**  
  - 如果需要显示 Markdown 语法中的特定字符，需要使用**反斜杠** `\` 进行转义    
  ```markdown
  \*\* 不加粗而是显示星号 \*\*  
  ```  
- **代码**  
  - 如果是段落内的一个代码片段，可以用**反引号** `` ` `` 包起来。如：`` `AAA` ``  
  - **转义**  
    - 当需要在代码中显示反引号，需要使用**双反引号包围单反引号**  
    - 需要显示多个反引号则使用**更多反引号**包围。如：``` ``AB`C`DE`` ```  
  - **代码区块**  
    - 使用 4 个空格或者一个制表符（Tab 键）  
    - 使用三反引号 ```` ``` ```` 包裹一段代码（**更常用**）  
    - 可在三反引号后添加语言标识符启用**语法高亮**功能  
    ````markdown  
    ```markdown  
    - AAA  
    - BBB  
    ```  
    [CCC](DDD)  
    ```` 
    - 当同时使用列表/嵌套列表和代码区块时，**应将代码区块与相应的列表对齐缩进**  
    - 如果遇到代码区块无法对齐相应列表的情况，可以添加**语言标识符**  
    `text` 可以不启用高亮只进行对齐  
    ````markdown
    - AAA
      - BBB
      ```text
      abc
      def
      ```
    ````
    - 不要在**嵌套列表之间**插入代码块，会影响嵌套逻辑判断  
    错误写法：  
    ````markdown
    - AAA
    ```
    abcd
    ```
      - BBB
      - CCC
    ````  

---
## 使用 Git 提交至 GitHub 仓库并部署  
[*Git 命令文档*](https://git-scm.com/docs)  
[*Cloudflare 部署文档及脚本*](https://gohugo.io/host-and-deploy/host-on-cloudflare/)
1. **配置 GitHub 仓库关联**  
    - **检查云端仓库地址是否已绑定**  
    ```cmd
    git remote -v
    ```
    正常应返回：  
    `origin https://github.com/用户名/仓库名.git (fetch)`  
    `origin https://github.com/用户名/仓库名.git (push)`  
    - **检查身份标识**  
    ```cmd
    git config --global user.name
    ```
    ```cmd
    git config --global user.email
    ```
    正常应返回 GitHub 名字和邮箱  
2. **查看缓存区状态**  
```cmd
git status
```
3. **将遗漏文件加入缓存区**  
```cmd
git add content/posts/my-new-post/
```
4. **记录本地仓库的修改**  
```text
git commit -m "更新博客文章"
```
`-m` 指添加提交备注  

5. **推送至 GitHub 仓库**  
```cmd
git push
```  
- 第一次推送时  
```cmd
git push -u origin master
```
`-u` 指建立上游追踪关系，让本地的 master 分支与云端的 master 分支联系起来  
master 为主分支的名称，也可能是 main  
以后推送可以只用 `git push`  

---
## 火狐浏览器快捷键  
~~*远离鼠标码字*~~  
- 关闭标签页 `Ctrl + W`  
- 切换最近标签页 `Ctrl + Tab`  
- 按对应位置切换标签页 `Ctrl + 数字键`  
- 跳到搜索栏 斜杠键 `/`  
- 选择页面按钮 `Tab`  
`Shift + Tab` 反选  
`↵` 确定  
- 转到上一页 `Alt + ←`  
- 启用或禁用光标浏览 `F7`  
可在页面中插入闪烁的选择光标  