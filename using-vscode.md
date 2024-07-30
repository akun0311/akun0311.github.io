# 基于VSCode进行C/C++项目开发【Makefile, C/C++, Clangd, Kconfig, No Cmake】


# 阅读提示

 - 本文对于在VS Code环境下使用C/C++项目开发进行一定的引导
   内容包括Visual Studio Code(简称VS Code), Ubuntu22.04 
 - 插件介绍: **Kconfig, C/C++ , clangd**。
 - 发布时间: 2024年7月

# 一、Visual Studio Code（简称vscode）

##  1.1 Vscode介绍

>(官网介绍）Visual Studio Code is a lightweight but powerful source code editor which runs on your desktop and is available for Windows, macOS and Linux. It comes with built-in support for JavaScript, TypeScript and Node.js and has a rich ecosystem of extensions for other languages and runtimes (such as C++, C#, Java, Python, PHP, Go, .NET). 
>
>(来源网络）VScode全称是Visual Studio Code，是微软推出的一个跨平台的编辑器，能够在windows、Linux、IOS等平台上运行，通过安装一些插件可以让这个编辑器变成一个编译器。 VSCode支持C++、Python、Java、C#、Go等多种语言，功能强大、插件丰富并且启动速度极快，值得每个开发人员尝试一把！

 - vscode是一款代码编辑器，但它并不是一个集成开发环境IDE **(不清楚两者区别? STFW )**。
   因此需要用户根据自己的需求配置各种开发工具和插件，以便支持不同的编程语言、构建系统和调试功能。
 - **了解更多: STFW**

## 1.2 Vscode安装与使用

 - 下载安装：[vscode官网](https://code.visualstudio.com/)

 - 使用介绍：[vscode官方文档](https://code.visualstudio.com/docs)

 - 常见问题：[vscode官网常见问题](https://code.visualstudio.com/docs/supporting/faq)

 - 了解更多：**STFW、STFM** 

 - 一些搜索关键词： **所有搜索词前面可适当加上一个`vscode`, 不一定要和搜索词完全相同，含义相同即可**    

   >vscode介绍，vscode新手必备， 如何使用vscode编写代码/开发项目
   >插件是什么，如何安装插件，中文插件，主题插件，透明背景
   >编辑器字体大小，终端字体大小，快捷键，自定义快捷键
   >.vscode文件夹是什么，settings.json是什么，launch.json是什么, vscode的配置文件， vscode的json文件
   >全局配置和用户配置， 工作区
   >vscode预定义变量，环境变量(STFM: Variables Reference）
   >vscode提高效率/提高生

- 小彩蛋: vscode的可执行程序叫做code,  试试在命令行中输入`code $NEMU_HOME`

# 二、基于vscode的C/C++项目开发

>更多内容请搜索:  VScode的C/C++插件推荐

## 2.1 Makefile插件

- 作者通过手动输入`make`命令来构建项目，因此**没有使用**任何基于Makefile的自动编译/运行工具(如`Makefile Tools`)

## 2.2 Konfig插件

- 作者使用`kconfig`插件为Kconfig配置提供`语法高亮`功能

## 2.3 C/C++语言插件

> C/C++插件和clangd插件两者之间有冲突，一般是选择其中一个使用

### （1）C/C++插件

- C/C++插件介绍

  >C/C++`插件是vscode中微软官方发布的一款用于C/C++语言开发的扩展插件，插件提供了丰富的代码编辑、调试、语法高亮、自动完成以及代码导航等特性，可以帮助开发者更高效地进行C++项目开发。

- vscode里使用C/C++插件

  1. 下载C/C++插件: 在vscode扩展功能里面搜索`C/C++`下载即可
  2. 该插件**安装简单，使用方便，基本可以做到开箱即用，所以安装完之后，无须任何配置，就可以直接使用了**
  3. 使用该插件时，可能会出现代码下面有红色波浪线的情况，有两种解决方案

     - 在`c_cpp_properties.json`文件里面配置头文件搜索路径等方式消除红色波浪线

     - 在`settings.json`文件里面使用`"C_Cpp.errorSquiggles": "disabled"`语句禁用错误提示
  4. 可能存在的问题

    > 问题: 配置文件是什么? 应该往哪个配置文件里面写代码? 配置文件里面的代码要如何写? 配置文件里面内容的含义? 	
    >
    > 解答: STFW

### （2）clangd（重点内容）

- **clangd介绍**

  >1. (官网介绍）clangd understands your C++ code and adds smart features to your editor: code completion, compile errors, go-to-definition and more. clangd is a language server that can work with many editors via a plugin.
  >2. (来源网络）clangd是一个开源的语言服务器，专门为C++开发者提供强大的集成开发环境（IDE）特性。这个工具并不只是文档的简单呈现，而是致力于提升你的代码编写效率和代码质量，通过与各种文本编辑器集成，为开发过程带来智能化的支持
  >3. (来源网络）clangd基于著名的LLVM项目，利用了Clang编译器的先进技术和深度语法理解能力。它实现了语言服务器协议，允许与诸如VS Code、Emacs或Vim等流行编辑器无缝协作。


- **vscode里使用clangd插件**

  1. **下载clangd :[clangd官网](https://clangd.llvm.org/installation)** 

     >vscode用户可以通过vscode的扩展功能自动下载并安装clangd插件
     >若使用vscode 请注意clangd扩展设置里面的`Clangd:Path`是否和安装的路径一致

  2. 使用clangd实现代码跳转、分析等功能需要读取一个`compile_commands.json`文件
     由于我们的项目是使用Makefile来构建，而Makefile在构建过程中无法生成**该json**文件，
     关于这一点，clangd官网给我们提供了生成该json文件的方式

     **！！！网上的各种资料都不如[clangd官网](https://clangd.llvm.org/installation.html)写的清晰明了！！！**

     **！！！务必阅读[clangd官网](https://clangd.llvm.org/installation.html)了解clangd的更多信息！！！**

     **！！！务必阅读[clangd官网](https://clangd.llvm.org/installation.html)中关于`Project Setup及Other build systems, using Bear`的描述！！！**
       安装好clangd ，compile_commands.json文件**正确生成并且被正确识别后**，就能愉快的使用clangd进行项目开发了

  3. 可能存在的问题

     - **不同ubuntu系统使用bear的区别**(下面命令仅做演示两者区别, 具体要执行什么命令需要自己思考/STFW/STFM）
       -   ubuntu22.04: bear -- make -j 10 
       -   ubuntu20.04: bear make -j 10
       -   **如果**compile_commands.json的内容不太对, **那么使用bear命令前，先make clean 然后再使用bear + make构建**
     - **如果**clangd识别不到生成的compile_commands.json文件， **那么STFW/STFM**


  4. 探索clangd

     - 下面的代码片段是作者`settings.json`里面的clangd配置，内容也是**来源于网络**，**抛砖引玉以让大家DIY自己的配置**

     - **STFW : json文件数据格式， vscode的clangd配置**

       ```bash
         "clangd.detectExtensionConflicts": true,
         "clangd.serverCompletionRanking": true,    
         "clangd.arguments": [
         "--completion-style=detailed",
         "--background-index",
         "-j=12",
         "--clang-tidy",
         "--clang-tidy-checks=readability-*",
         "--all-scopes-completion",
         "--completion-style=detailed",
         "--header-insertion=iwyu",
         "--pch-storage=disk", 
         "--diagnostics-format=clangd",
         ],
       ```