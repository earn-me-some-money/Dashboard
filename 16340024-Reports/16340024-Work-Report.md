# Earn Me The Money! 后端开发报告

我在 Earn Me The Money! 项目中的职务为后端工程师，主要负责数据库、服务器部署、信息检索以及后台系统框架设计。

## 后端系统整体架构

1. **语言**：出于开发效率考虑，项目起始时我们对编程语言的选择有 Rust，Golang，JavaScript 等，首先出于运行速度的原因排除了 JavaScript, Python 等非 Native 语言，最后在 Golang 与 Rust 中选择了 Rust 的原因是 Rust 提供了更多优秀的语言特性、更快的运行速度以及优秀的编译时检查和更高的安全性，从而减少运行时 Debug 的难度。虽然 Rust 的学习曲线可能较为陡峭，但是整体来看是对有利于提升项目产品质量的。
2. **Web 框架**：在 Rust 语言中可选的 web 框架有 Actix-web, Rocket, warp 等。我们最后选择 Actix-web 的原因是其不需要使用 Nightly 版本的编译器，有着更加好的社区支持，有效利用了异步特性，并在 [benchmark](https://www.techempower.com/benchmarks/#section=data-r17&hw=ph&test=query) 中有很高的排名。
3. **ORM**：我们选择使用了 diesel ORM 来提供 MySQL 关系数据库的访问，选择的原因是 diesel 提供了便利且类型安全的数据库访问，并可简化部署时数据库迁移的工作。带来的缺点是由于 diesel 没有使用异步数据库驱动，可能会拖慢框架速度，这点也可在框架 benchmark 中看到。
4. **Docker**：我们选择了使用 Docker Compose 来进行程序的部署，通过 docker compose 可以方便地在服务器上设置必须的信息后一键部署，减少配置环境的麻烦。缺点是由于 Rust 语言编译时间较长，且 `crates.io` 在国内连接较差，需要通过阿里云的海外容器构建服务来进行容器构建，增长了每次更新所需要的时间。
5. **Web 服务器**：我们选择 Caddy 作为 Web 服务器，在项目中 Caddy 主要的用途是对程序的业务请求进行反向代理。相较于 NGINX，Caddy 提供了更简单的配置编写、更方便开启 HTTPS 的特点，但是由于阿里云会对未备案主机进行 80 端口截断，我们无法利用 Caddy 提供的自动申请 Let's Encrypt 证书功能，而只能通过 DNS Challenge 来申请。
6. **文本搜索**：在我们的项目中需要对用户任务的信息进行搜索，这方面我选用 Tantivy 库进行实现，它提供了许多类似于在 Java 项目中常用的 Lucene 库的功能。并通过结巴分词实现对中文文本的快速的搜索。