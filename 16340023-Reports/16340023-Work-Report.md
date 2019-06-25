# 使用Rust Actix_web框架进行服务端构建

> System Analsis course's personal work report

### 姓名：陈明亮

### 学号：16340023



## 何为Rust语言？

* Rust是一门系统编程语言，专注于安全，尤其是并发安全，支持函数式和命令式以及泛型等编程范式的多范式语言。Rust在语法上和C++类似，但是设计者想要在保证性能的同时提供更好的内存安全。 
* Rust最初是由Mozilla研究院的Graydon Hoare设计创造，然后在Dave Herman, Brendan Eich以及很多其他人的贡献下逐步完善的。




## 选择Rust语言进行服务端开发的优缺点

### 优点

1. Rust致力于成为优雅解决高并发和高安全性系统问题的编程语言 ，适用于大型场景，即创造维护能够保持大型系统完整的边界。这就导致了它强调安全，内存布局控制和并发的特点。
2. 标准Rust性能与标准C++性能不相上下，性能上优于大部分的动态语言。




### 缺点

1. 学习曲线较为陡峭，编程语言与编程标准采用静态编译型语言标准，同时编译时的检查相对于动态语言要严格得多，这要求开发者熟知Rust语法，严格遵循开发规范。
2. 服务端框架种类较多，不同框架之间的编程风格差异与应用场景差异较大，须根据需求进行框架的选取，额外学习对应框架的开发。




## Actix-Web框架

Actix-web 是 Rust 的一个简单、实用且极其快速的 Web 框架。
1. 支持 HTTP/1.x 和 HTTP/2.0 协议
2. Streaming 和 pipelining
3. Keep-alive 和慢请求处理
4. 客户端/服务器 WebSockets 支持
5. 透明内容压缩/解压缩（br、gzip、deflate）
6. 可配置的请求路由
7. Multipart 流
8. 静态资源
9. SSL 支持 OpenSSL 或native-tls
10. 中间件（Logger、Session、CORS、CSRF 等）
11. 包括异步 HTTP 客户端
12. 建立在 Actix actor 框架之上(Rust语言原生框架)




## 使用Cargo工具构建Actix-Web项目

1. cargo是一款很实用的Rust项目构建与包管理工具，安装完Rust编译环境之后(具体安装流程请点击：[传送门](https://www.rust-lang.org/learn/get-started))，cargo便可以直接使用命令行进行使用，新建项目命令如下：

   ```bash
   $ cargo new hello_world --bin
   ```

   cargo工具主要负责三个工作：

   * 构建`build`代码
   * 下载代码依赖`dependencies`的包装箱`crate`
   * 编译项目源码与包装箱`crate`

   检查当前命令行下可否正常使用cargo，通常查看其版本是否正常显示：

   ```bash
   $ cargo --version
   ```

   ​

2. 此时进入工程项目文件夹，发现有`src`文件夹以及`Cargo.toml`文件，分别用来放置项目源代码文件，以及管理项目所需的外部库文件。每当项目需要使用到现成的外部库时，我们都需要进入`Cargo.toml`进行项目依赖库的添加：

   ```toml
   [dependencies]
   'package_name' = 'package_version'
   ```

   在Rust中，这些外部依赖库通常被称为包装箱`crate`，类似于其他语言的库`library`或者`package`

   ​

3. 相应的依赖文件添加完成之后，我们便可以在`src`文件夹内创建对应的源程序文件`main.rs`，作为入门示例，编写`Rust`语言下的`hello-world`：

   ```rust
   fn main() {  
       println!("Hello, world!");  
   }
   ```

   当用户第一次`build`项目时，cargo会进行当前依赖的包装箱版本进行查找，并且记录到`Cargo.lock`中，在你第二次进行`build`操作时，直接读取`Cargo.lock`文件内部每个包装箱的版本号，而不是再去查明和记录所需依赖的版本号，这让你有了一个可重复的自动构建过程。

   ​

4. 返回项目根目录下，运行：

   ```bash
   $ cargo build
   ```

   编译当前完成的项目源文件，同时使用：

   ```bash
   $ cargo run
   ```

   运行编译完成的可执行文件

   ​


5. 很多时候会因为依赖库版本冲突问题，导致程序编译时出现较多错误，此时可以使用：

   ```bash
   $ cargo update
   ```

   升级当前低版本的依赖库。

   同时，`cargo`工具还附带另外一种特色用法，即格式化当前源程序中的代码风格，成为`Rust`的标准编码风格：

   ```bash
   $ cargo fmt
   ```

   ​

6. 关于对`Actix_web`框架项目的构建，实际上也只需要在`Cargo.toml`中添加对应的依赖项，即可在代码编写中使用相应的函数接口进行编程：

   ```toml
   [dependencies]
   actix-web = {version = "1.0", features = ["ssl", "client"] }
   actix-multipart = "0.1.2"
   ```

   ​

7. 关于cargo配置国内镜像源的补充：在进行依赖文件下载时，网速的高低起到决定性的作用，但由于Rust官方源网站被墙得过于严重，所以为了提升下载速度，需要更换cargo的下载镜像源：

   * 在`$HOME/.cargo/config`中添加以下内容，切换为中科大镜像源：

     ```toml
     [source.crates-io]
     replace-with = 'ustc'

     [source.ustc]
     registry = "git://mirrors.ustc.edu.cn/crates.io-index"
     ```

     ​

   * 更新配置文件，使其生效：

     ```bash
     $ source $HOME/.cargo/config
     ```

     ​

   * 如果所处的环境中`git`协议不能使用，可以更改为`https`协议：

     ```toml
     registry = "https://mirrors.ustc.edu.cn/crates.io-index"
     ```

     ​
