# 环境设置

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

### 配置

在 .pre-commit-config.yaml 文件中，配置你要执行的所有工具。

```yaml
# ========================================================
# pre-commit 配置文件
# 作用：定义在 Git 提交前自动运行的检查工具（钩子）
# 文件名必须为 `.pre-commit-config.yaml`
# ========================================================

# 核心配置节：定义需要使用的钩子仓库集合
repos:
  # ----------------------------
  # 示例 1: 基础代码格式检查工具（pre-commit 官方钩子库）
  # ----------------------------
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0  # 固定版本号，避免更新导致意外行为
    hooks:
      - id: trailing-whitespace  # 检查并修复行尾空格
      - id: end-of-file-fixer    # 确保文件以换行符结尾
      - id: check-yaml           # 校验 YAML 文件语法
        args: [ --unsafe ]         # 传递额外参数（允许不安全的 YAML 解析）
      - id: check-json           # 校验 JSON 文件语法
      - id: check-added-large-files  # 阻止提交大文件（默认 >500KB）
        args: [ --maxkb=1024 ]     # 自定义最大文件大小

  # ----------------------------
  # 示例 2: Python 代码格式化（使用 black）
  # ----------------------------
  - repo: https://github.com/psf/black
    rev: 23.3.0
    hooks:
      - id: black
        name: Black Code Formatter  # 自定义钩子名称
        language_version: python3.9 # 指定 Python 版本
        types: [ python ]            # 仅检查 Python 文件
        exclude: ^tests/           # 排除 tests 目录

  # ----------------------------
  # 示例 3: JavaScript/TypeScript 代码检查（使用 ESLint）
  # ----------------------------
  - repo: https://github.com/pre-commit/mirrors-eslint
    rev: v8.55.0
    hooks:
      - id: eslint
        args: [ --fix ]             # 自动修复可修复的错误
        files: \.(js|ts)x?$       # 匹配 JS/TS/JSX/TSX 文件
        types: [ file ]

  # ----------------------------
  # 示例 4: 本地自定义钩子（无需远程仓库）
  # ----------------------------
  - repo: local  # 本地钩子标识
    hooks:
      - id: run-unit-tests
        name: "运行单元测试"       # 自定义显示名称
        entry: pytest tests/     # 执行的命令
        language: system         # 使用系统环境（不自动安装依赖）
        files: \.py$             # 仅当 Python 文件变更时触发
        pass_filenames: false    # 不传递文件名给命令

  # ----------------------------
  # 示例 5: Git 提交消息规范化（使用 commitizen）
  # ----------------------------
  - repo: https://github.com/commitizen-tools/commitizen
    rev: v3.12.0
    hooks:
      - id: commitizen
        stages: [ commit-msg ]     # 指定在提交消息阶段触发

# ========================================================
# 可选高级配置
# ========================================================

# 全局排除文件（支持正则表达式）
exclude: |
  (?x)^(
    .*/\.git/.*|       # 忽略 .git 目录
    .*/node_modules/.* # 忽略 node_modules
    .*/test_data/.*    # 忽略测试数据目录
  )$

# 默认执行阶段（可选值：commit, commit-msg, push, manual）
default_stages: [ commit ]

# 是否允许跳过钩子（通过 --no-verify 参数）
fail_fast: false       # false：遇到错误继续执行其他钩子
```

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

### 配置文件

deny.toml文件中调整配置

```yaml
# 此模板包含所有可能的配置节及其默认值

# 注意，所有接受检查级别的字段有以下可能值：
# * deny - 产生错误并导致检查失败
# * warn - 产生警告，但检查不会失败
# * allow - 不产生警告或错误，但在某些情况下会生成备注

# 本模板提供的值为默认值，当您自己的配置中未指定某节或字段时将使用这些默认值

# 根选项

# graph表配置依赖图的构建方式，从而决定检查针对哪些crate
[graph]
# 如果指定了1个或多个目标三元组（可选含target_features），则运行`cargo deny check`时
# 只会检查指定的目标。这意味着，如果某个包仅在特定目标下作为依赖使用（例如`nix`crate
# 仅在`target_family = "unix"`配置下使用），而在此列表中仅包含Windows目标，则nix crate
# 及其独有依赖将被忽略，因为此目标列表实际上声明了您要构建的目标平台。
targets = [
    # 三元组可以是任意字符串，但只有rustc内置的目标三元组（截至1.40版本）能验证实际配置表达式
    #"x86_64-unknown-linux-musl",
    # 也可以指定特定目标启用的target_features。当前不会验证target_features是否符合目标架构支持的有效特性
    #{ triple = "wasm32-unknown-unknown", features = ["atomics"] },
]
# 创建作为检查依据的依赖图时，此字段可用于从图中修剪crate，使其脱离cargo-deny的检查范围。
# 这是极其强力的手段，因为如果一个crate被修剪，其所有依赖也将被修剪（除非这些依赖通过其他未被修剪的crate连接）。
# 因此应谨慎使用。标识符使用[Package ID规范]
# (https://doc.rust-lang.org/cargo/reference/pkgid-spec.html)
#exclude = []
# 如果为true，元数据收集将使用`--all-features`。注意如果设为true则无法关闭，
# 如需条件式启用建议通过命令行传递`--all-features`
all-features = false
# 如果为true，元数据收集将使用`--no-default-features`。注意事项同`all-features`
no-default-features = false
# 如果设置，收集元数据时将启用这些特性。命令行指定的`--features`会覆盖此选项
#features = []

# output表配置诊断信息的输出方式
[output]
# 在包含特性的诊断信息中输出依赖图时，此选项指定添加特性边的深度。
# 由于图表可能非常庞大，从crate到所有根节点的特性路径可能过于冗长，故设此选项。
# 可通过命令行`--feature-depth`覆盖此设置
feature-depth = 1

# 以下节在运行`cargo deny check advisories`时生效
# 完整文档请参考：
# https://embarkstudios.github.io/cargo-deny/checks/advisories/cfg.html
[advisories]
# 安全通告数据库的存储路径
#db-path = "$CARGO_HOME/advisory-dbs"
# 使用的安全通告数据库URL
#db-urls = ["https://github.com/rustsec/advisory-db"]
# 要忽略的安全通告ID列表。注意被忽略的通告仍会在遇到时输出备注
ignore = [
    #"RUSTSEC-0000-0000",
    #{ id = "RUSTSEC-0000-0000", reason = "可指定忽略该通告的原因" },
    #"a-crate-that-is-yanked@0.1.1", # 也可以选择忽略已撤回的crate版本
    #{ crate = "a-crate-that-is-yanked@0.1.1", reason = "可指定忽略已撤回crate的原因" },
]
# 如果为true，cargo deny将使用git可执行文件获取安全通告数据库。
# 如果为false，则使用内置git库。设为true有助于处理cargo-deny不支持的认证场景。
# 详见Git认证文档
#git-fetch-with-cli = true

# 以下节在运行`cargo deny check licenses`时生效
# 完整文档请参考：
# https://embarkstudios.github.io/cargo-deny/checks/licenses/cfg.html
[licenses]
# 显式允许的许可证列表
# SPDX许可证列表请参考：https://spdx.org/licenses/
# [可选值：任意SPDX 3.11短标识符（+可选例外条款）]
allow = [
    #"MIT",
    #"Apache-2.0",
    #"Apache-2.0 WITH LLVM-exception",
]
# 从许可证文本检测许可证的置信度阈值。
# 值越高，许可证文本必须与标准SPDX许可证文件内容越接近。
# [可选值：0.0到1.0之间的任意值]
confidence-threshold = 0.8
# 允许为特定crate设置例外许可证（不全局允许）
exceptions = [
    # 每个条目包含crate、版本约束及其允许的许可证
    #{ allow = ["Zlib"], crate = "adler32" },
]

# 有些crate没有（易于）机器可读的许可证信息，可通过clarify条目手动指定
#[[licenses.clarify]]
# 应用澄清的包规范
#crate = "ring"
# 该crate的SPDX许可证表达式
#expression = "MIT AND ISC AND OpenSSL"
# 用于验证许可证表达式的源文件（内容哈希匹配时生效）
#license-files = [
# 每个条目是crate内的相对路径及其内容哈希
#{ path = "LICENSE", hash = 0xbd0eed23 }
#]

[licenses.private]
# 如果为true，则忽略未发布或仅发布到私有注册表的工作区crate。
# 标记crate为未发布的方法请参考：
# https://doc.rust-lang.org/cargo/reference/manifest.html#the-publish-field
ignore = false
# 您可能发布crate到的私有注册表。如果crate仅发布到这些注册表且ignore为true，则不检查其许可证
registries = [
    #"https://sekretz.com/registry
]

# 以下节在运行`cargo deny check bans`时生效
# 完整文档请参考：
# https://embarkstudios.github.io/cargo-deny/checks/bans/cfg.html
[bans]
# 检测到同一crate多版本时的检查级别
multiple-versions = "warn"
# crate版本要求为`*`时的检查级别
wildcards = "allow"
# 多版本crate的依赖图高亮方式
# * lowest-version - 高亮到最低版本的路径
# * simplest-path - 高亮边数最少的路径
# * all - 同时使用上述两种方式
highlight = "all"
# 工作区成员crate默认特性的检查级别，可通过具体crate配置覆盖
workspace-default-features = "allow"
# 外部crate默认特性的检查级别，可通过具体crate配置覆盖
external-default-features = "allow"
# 白名单crate（慎用！）
allow = [
    #"ansi_term@0.11.0",
    #{ crate = "ansi_term@0.11.0", reason = "可指定允许原因" },
]
# 黑名单crate
deny = [
    #"ansi_term@0.11.0",
    #{ crate = "ansi_term@0.11.0", reason = "可指定禁止原因" },
    # 可指定包装crate，允许被禁crate作为其直接依赖时使用
    #{ crate = "ansi_term@0.11.0", wrappers = ["this-crate-directly-depends-on-ansi_term"] },
]

# 特性允许/禁止列表
# 每个条目包含crate名称和版本范围（未指定版本则匹配所有版本）
#[[bans.features]]
#crate = "reqwest"
# 禁止的特性
#deny = ["json"]
# 允许的特性
#allow = [
#    "rustls",
#    "__rustls",
#    "__tls",
#    "hyper-rustls",
#    "rustls",
#    "rustls-pemfile",
#    "rustls-tls-webpki-roots",
#    "tokio-rustls",
#    "webpki-roots",
#]
# 如果为true，允许的特性必须完全匹配已启用的特性集（此时设置deny无意义）
#exact = true

# 跳过重复检测的crate/版本
skip = [
    #"ansi_term@0.11.0",
    #{ crate = "ansi_term@0.11.0", reason = "可指定无法更新/移除的原因" },
]
# 跳过整个依赖树的检测（默认深度无限）
skip-tree = [
    #"ansi_term@0.11.0", # 跳过该crate及其所有传递依赖
    #{ crate = "ansi_term@0.11.0", depth = 20 },
]

# 以下节在运行`cargo deny check sources`时生效
# 完整文档请参考：
# https://embarkstudios.github.io/cargo-deny/checks/sources/cfg.html
[sources]
# 遇到未允许注册表来源时的检查级别
unknown-registry = "warn"
# 遇到未允许git仓库来源时的检查级别
unknown-git = "warn"
# 允许的注册表URL列表（未指定时默认允许crates.io）
allow-registry = ["https://github.com/rust-lang/crates.io-index"]
# 允许的git仓库URL列表
allow-git = []

[sources.allow-org]
# 允许github.com组织的git来源
github = []
# 允许gitlab.com组织的git来源
gitlab = []
# 允许bitbucket.org组织的git来源
bitbucket = []
```

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

### 配置文件

typos.toml文件中调整配置

```yaml
# ========================================================
# cargo typos 配置文件
  # 作用：检查代码中的拼写错误（如变量名、注释、文档等）
  # 官方文档：https://github.com/crate-ci/typos
  # ========================================================

  # 基础配置
  [ default ]
  # 是否将建议的更正直接写入文件（默认仅报告错误）
  write = false
  # 检查的文件类型（支持通配符）
  files = ["**/*.rs", "**/*.md", "**/*.txt"]
  # 排除的文件或目录（支持通配符）
  exclude = [
  "**/target/",       # 忽略构建目录
  "**/node_modules/", # 忽略前端依赖目录
  "**/*.lock",        # 忽略锁文件
]

  # 自定义词典配置
  [ default.dict ]
  # 添加项目专用词汇（避免误报）
  extend-words = [
  "wasm",      # WebAssembly 缩写
  "Rustacean", # Rust 社区术语
  "async",     # 异步关键字
]
  # 忽略的词汇（不检查这些词）
  ignore-words = [
  "lorem",     # 示例文本中的占位词
  "ipsum",     # 示例文本中的占位词
  "dolor",     # 示例文本中的占位词
]

  # 语言特定配置（按文件扩展名分组）
  [ default.languages ]
  # 配置 Markdown 文件的检查规则
  "*.md" = { check-strings = true, check-comments = false }

  # 配置 Rust 文件的检查规则
  "*.rs" = {
  check-strings = true,   # 检查字符串字面量
  check-comments = true,  # 检查注释
  check-docs = true,      # 检查文档注释（/// 和 //!）
  skip-identifiers = ["u8", "i32"]  # 跳过特定标识符（如类型名）
}

  # ========================================================
  # 高级配置
  # ========================================================

  # 正则表达式匹配忽略规则（支持复杂模式）
  [ default.overrides ]
  # 忽略所有全大写的缩写词（如 "HTTP"、"JSON"）
  '^[A-Z]{2,}$' = "ignore"
  # 忽略特定模式的变量名（如带下划线的蛇形命名）
  '^[a-z_]+$' = "ignore"

  # 自定义替换规则（自动更正常见拼写错误）
  [ default.replace ]
  "seperate" = "separate"    # 将 "seperate" 替换为 "separate"
  "recieve" = "receive"      # 将 "recieve" 替换为 "receive"
  "adn" = "and"              # 将 "adn" 替换为 "and"

  # 禁用默认词典中的部分规则
  [ default.disabled ]
  # 禁用对英式拼写的检查（如 "colour" vs "color"）
  variants = ["british"]

  # ========================================================
  # 使用说明
  # 1. 保存此文件为 `typos.toml` 到项目根目录
  # 2. 运行检查：
  #    cargo typos --config typos.toml
  # 3. 自动修复错误（需启用 write = true）：
  #    cargo typos --write
# ========================================================
```

## 安装 git-cliff

git cliff 是一个生成 changelog 的工具。

```bash
# 安装
cargo install git-cliff
```

参考： [https://git-cliff.org/docs/](https://git-cliff.org/docs/)

### 配置文件

根目录下cliff.toml文件中调整配置

```yaml
# ========================================================
# git-cliff 配置文件
# 作用：基于 Git 提交历史生成结构化的变更日志
# 官方文档：https://git-cliff.org/docs/configuration
# ========================================================

# 基础配置
[changelog]
# 生成的日志文件名（默认：CHANGELOG.md）
file = "CHANGELOG.md"
# 日志文件头部内容（支持模板语法）
header = """
# 变更日志\n
此项目遵循[语义化版本](https://semver.org/)。\n
"""
# 日志条目模板（控制每条提交的展示格式）
body = """
## {{ version }}\n
{% if timestamp %}_发布日期：{{ timestamp | date(format="%Y-%m-%d") }}_ {% endif %}\n
{% for group, commits in commits | group_by(attribute="group") %}
### {{ group | upper_first }}\n
{% for commit in commits %}
- {{ commit.message | upper_first }} ([{{ commit.id | truncate(length=7, end="") }}]({{ commit.link }}))\n
{% endfor %}
{% endfor %}
"""
# 日志尾部内容（如协议声明）
footer = """
---
_此文件由 [git-cliff](https://git-cliff.org) 自动生成_"""

# Git 提交解析规则
[git]
# 提交类型到日志分组的映射（可自定义）
[git.conventional_commit]
types = [
    { type = "feat", group = "新增功能" },
    { type = "fix", group = "问题修复" },
    { type = "docs", group = "文档更新" },
    { type = "style", group = "代码样式" },
    { type = "refactor", group = "代码重构" },
    { type = "perf", group = "性能优化" },
    { type = "test", group = "测试相关" },
    { type = "chore", group = "维护任务" },
]
# 匹配提交作用域的正则表达式（如 `feat(ui): ...` 中的 'ui'）
scope_pattern = "(\\w+)"  
# 匹配提交中重大变更的正则表达式（如包含 `BREAKING CHANGE:` 的提交）
breaking_pattern = "^BREAKING CHANGE:"  

# 提交过滤规则
[commit_filters]
# 包含的提交类型（为空则包含所有）
include = ["feat", "fix", "perf", "refactor", "BREAKING CHANGE"]
# 排除的提交类型（如忽略 chore 和 docs）
# exclude = ["chore", "docs"]

# 版本生成规则
[bump]
# 根据提交类型自动提升版本号（语义化版本）
map = [
    { breaking = true, bump = "major" },  # 重大变更 -> 主版本号+1
    { type = "feat", bump = "minor" },     # 新增功能 -> 次版本号+1
    { type = "fix", bump = "patch" },      # 问题修复 -> 修订号+1
]

# 模板变量（用于自定义占位符）
[template]
# 版本号前缀（如 "v" 会生成 "v1.0.0"）
version_prefix = "v"
# 提交链接格式（需适配代码托管平台）
commit_link = "https://github.com/your-username/your-repo/commit/{{ id }}"

# 全局忽略规则（支持正则表达式）
[ignore]
commits = [
    "^Merge branch",    # 忽略合并提交
    "^Update README",   # 忽略特定提交消息
]
files = [
    ".*\\.md",          # 忽略 Markdown 文件变更
    "package-lock\\.json",  # 忽略 lock 文件
]

# ========================================================
# 使用说明
# 1. 保存此文件为 `cliff.toml` 到项目根目录
# 2. 生成变更日志：
#    git-cliff --output CHANGELOG.md
# 3. 生成特定版本的日志（如从 v1.0.0 开始）：
#    git-cliff --tag v1.0.0..HEAD
# ========================================================
```

## 安装 cargo nextest

cargo nextest 是一个 Rust 增强测试工具。

```bash
# 安装
cargo install cargo-nextest
# 测试
cargo nextest run
```