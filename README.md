# 生产环境配置

## 安装 VSCode 插件

* crates: Rust 包管理
* Even Better TOML: TOML 文件支持
* Better Comments: 优化注释显示
* Error Lens: 错误提示优化
* GitLens: Git 增强
* Github Copilot: 代码提示
* indent-rainbow: 缩进显示优化
* Prettier - Code formatter: 代码格式化
* REST client: REST API 调试
* rust-analyzer: Rust 语言支持
* Rust Test lens: Rust 测试支持
* Rust Test Explorer: Rust 测试概览
* TODO Highlight: TODO 高亮
* vscode-icons: 图标优化
* YAML: YAML 文件支持

## 安装 cargo generate

cargo generate 是一个用于生成项目模板的工具。它可以使用已有的 github repo 作为模版生成新的项目。

```bash
# 安装
cargo install cargo-generate
# 使用
cargo generate github项目模型
cargo generate wang-wenxiangs/rust_template
```

参考：[https://github.com/cargo-generate/cargo-generate](https://github.com/cargo-generate/cargo-generate)

## 安装 pre-commit

pre-commit 是一个代码检查工具，可以在提交代码前进行代码检查，需要安装 python。

```bash
# 安装
pip install pre-commit
# 安装钩子到 Git
pre-commit install
# 手动运行所有检查
pre-commit run --all-files
```

参考： [https://pre-commit.com/](https://pre-commit.com/)

## 安装 Cargo deny

Cargo deny 是一个 Cargo 插件，可以用于检查依赖的安全性。

```bash
# 安装
cargo install --locked cargo-deny
# 验证
cargo deny --version
# 初始化配置文件
cargo deny init
# 检查依赖
cargo deny check
```

参考：[ https://github.com/EmbarkStudios/cargo-deny]( https://github.com/EmbarkStudios/cargo-deny)

自定义配置: [https://embarkstudios.github.io/cargo-deny/](https://embarkstudios.github.io/cargo-deny/)

## 安装 typos

typos 是一个拼写检查工具。

```bash
# 安装
cargo install typos-cli
# 验证
typos --version
# 检查拼写
typos
```

参考： [https://github.com/crate-ci/typos](https://github.com/crate-ci/typos)

## 安装 git-cliff

git cliff 是一个生成 changelog 的工具。

```bash
# 安装
cargo install git-cliff
```

参考： [https://git-cliff.org/docs/](https://git-cliff.org/docs/)

## 安装 cargo nextest

cargo nextest 是一个 Rust 增强测试工具。

```bash
# 安装
cargo install cargo-nextest
# 测试
cargo nextest run
```