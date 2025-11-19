`.gitignore` 是 Git 版本控制系统中用于指定不需要纳入版本管理的文件或目录的配置文件。它的作用是告诉 Git 哪些文件应该被忽略，不被跟踪（track），从而避免这些文件被误提交到代码仓库中，保持仓库的整洁和专注。

### **核心作用**

1. **排除临时文件**：如编译生成的 `.class`、`.o` 文件，日志文件（`.log`），缓存文件等。
2. **排除个人配置**：如 IDE 生成的项目配置（`.idea/`、`.vscode/`）、本地环境变量文件（`.env.local`）等。
3. **保护敏感信息**：如包含密码、密钥的文件（`config.ini` 中的私密配置），避免泄露到公开仓库。
4. **减少仓库体积**：排除大文件（如数据库备份、视频等），避免仓库臃肿。

### **基本规则**

`.gitignore` 文件通过**模式匹配**来指定需要忽略的文件 / 目录，常用规则如下：

|规则示例|说明|
|---|---|
|`*.log`|忽略所有以 `.log` 结尾的文件|
|`tmp/`|忽略名为 `tmp` 的目录（及其下所有内容）|
|`build`|忽略名为 `build` 的文件或目录（若为目录，需确保末尾无 `/` 时也能匹配）|
|`!important.log`|否定匹配：不忽略 `important.log`（覆盖前面的 `*.log` 规则）|
|`src/*.txt`|只忽略 `src` 目录下的 `.txt` 文件，不忽略子目录中的 `.txt`|
|`src/**/*.txt`|忽略 `src` 目录及其所有子目录中的 `.txt` 文件（`**` 表示任意层级目录）|
|`# 这是注释`|以 `#` 开头的行是注释，会被 Git 忽略|
|`*.swp`|忽略 Vim 编辑器的临时交换文件|
|`.idea/`|忽略 IntelliJ IDEA 的配置目录|

### **使用方式**

1. **创建 `.gitignore` 文件**
    
    在 Git 仓库的根目录（与 `.git` 文件夹同级）创建一个名为 `.gitignore` 的文件，然后写入上述规则。
    
    示例内容：
```gitignore
    # 忽略日志文件
    *.log
    
    # 忽略编译目录
    dist/
    build/
    
    # 忽略 IDE 配置
    .idea/
    .vscode/
    *.sublime-*
    
    # 忽略环境变量文件
    .env.local
    .env.*.local
    
    # 不忽略关键配置（否定规则）
    !.env.example
```
    
2. **生效范围**
    
    - `.gitignore` 只对**未被跟踪**的文件有效。如果文件已被 `git add` 或 `git commit` 过（即已纳入版本管理），修改 `.gitignore` 不会自动忽略它，需要先移除跟踪：
        

        
```bash
# 移除文件的跟踪（但不删除本地文件）
git rm --cached 文件名
# 提交修改
git commit -m "移除已跟踪的忽略文件"
```


### **全局 `.gitignore`**

如果某些规则（如个人编辑器配置）需要在所有 Git 仓库中生效，可以配置**全局 `.gitignore`**：

1. 创建全局忽略文件（如 `~/.gitignore_global`）。
2. 配置 Git 识别该文件：
```bash
   git config --global core.excludesfile ~/.gitignore_global
```


### **注意事项**

- 区分**文件**和**目录**：目录名后加 `/`（如 `tmp/`）可明确指定忽略目录，避免误判为文件。
- 否定规则（`!`）的优先级：后出现的规则会覆盖前面的冲突规则。
- 不要忽略必要的配置模板：例如 `.env.example`（不含敏感信息的示例配置）应该提交，方便他人参考。
- 部分项目有通用模板：如 Java 项目可使用 `*.class`，Python 项目可使用 `__pycache__/`、`*.pyc` 等，可参考 [GitHub 的 `.gitignore` 模板库](https://github.com/github/gitignore)。

通过合理配置 `.gitignore`，可以有效提升 Git 仓库的管理效率，避免不必要的文件干扰。


下面是用于obsidian的.gitignore文件，复制进去就行了

```gitignore
# ==============================================
# Obsidian 自身生成的文件/目录（无需跟踪）
# ==============================================
# 缓存目录（存储索引、搜索缓存等）
.obsidian/cache/
# 临时文件（如自动保存的草稿）
.obsidian/workspace-mobile.json
.obsidian/workspace.json.bak
.obsidian/*.tmp
# 插件临时数据（部分插件会生成缓存）
.obsidian/plugins/*/data/
.obsidian/plugins/*/cache/
# 本地设置（如窗口布局、折叠状态，仅本地有效）
.obsidian/workspace.json
.obsidian/appearance.json
.obsidian/keyboard.json
.obsidian/starred.json

# ==============================================
# 可能的敏感信息或本地配置
# ==============================================
# 隐私相关插件数据（如密码管理器、本地API密钥）
.obsidian/plugins/privacy/
# 本地附件缓存（如果使用云同步附件，可忽略）
.obsidian/attachments/
# 自定义CSS缓存（如果CSS源文件已跟踪，缓存无需提交）
.obsidian/snippets/*.css.map

# ==============================================
# 通用编辑器/系统文件（避免跨平台干扰）
# ==============================================
# 操作系统临时文件
.DS_Store          # macOS
Thumbs.db          # Windows
*.swp              # Vim 临时文件
*.swo              # Vim 临时文件
*~                 # 部分编辑器的备份文件

# 编辑器配置（如果多人协作，建议忽略个人IDE配置）
.idea/             # IntelliJ/IDEA
.vscode/           # VS Code
*.sublime-*        # Sublime Text

# ==============================================
# 可选：忽略特定类型的附件（根据需求调整）
# ==============================================
# 如果不需要跟踪大文件（如视频、大型压缩包）
# *.mp4
# *.zip
# *.rar

# ==============================================
# 注意：以下文件通常需要保留，不要忽略！
# ==============================================
# .obsidian/config.json       # 核心配置（如仓库名称、默认视图）
# .obsidian/plugins/          # 插件列表（确保多人协作时插件一致）
# 所有 .md 笔记文件           # 核心内容，默认不忽略
# 附件目录（如 attachments/）  # 若手动管理附件，需保留
```