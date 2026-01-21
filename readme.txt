Git is a version control system.
Git is free software.
12122

Git is a distributed version control system.
Git is free software.

Git is a distributed version control system.
Git is free software.
Git is a distributed version control system.
Git is free software.
Git is a distributed version control system.
Git is free software.


1、先打开 Windows 终端（CMD/PowerShell/Git Bash），切换到你的 Git 仓库根目录：cd C:\Users\15041\learngit
2、步骤 2：创建 / 准备要添加的文件夹，比如你要添加一个叫 docs 的文件夹：mkdir docs，# 如果文件夹是空的，创建占位文件（否则 Git 不会追踪空文件夹）
echo. > docs/.gitkeep  # CMD 命令，创建空的 .gitkeep 文件
3、步骤 3：将文件夹添加到 Git 暂存区
这是 “添加文件夹” 的核心步骤，有两种方式：# 方式 1：添加指定文件夹（推荐，精准添加）
git add docs/  # 末尾的 / 表示添加整个文件夹，包含里面所有文件

# 方式 2：添加当前目录下所有未追踪的文件/文件夹（适合批量添加）
git add .
验证：执行 git status，会看到类似提示（说明文件夹已被暂存）：
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   docs/.gitkeep

4、步骤 4：提交文件夹到 Git 仓库
添加到暂存区后，必须提交才能真正纳入版本管理：
git commit -m "add docs folder"  # 提交说明要清晰，描述添加的文件夹，执行后会看到类似反馈，说明提交成功：
[master xxxxxx] add docs folder
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 docs/.gitkeep
5、步骤 5：验证文件夹已被 Git 追踪
执行以下命令，查看提交记录或文件列表，确认文件夹已被纳入管理：
# 查看仓库内的文件/文件夹结构
git ls-files  # 会显示 docs/.gitkeep
# 查看提交历史，确认提交记录
git log --oneline

6、在vscode中git的用法是在vscode中做完修改后，单击changes的对勾，提交修改并保存，然后，本地的git就会保存新版本。

1. 问题根源：CMD 的 cd 命令跨盘符特性
在 Windows CMD 中：
如果你当前在 C: 盘（比如 C:\Users\15041\learngit），直接执行 cd E:\metallographic_analyzer_v3.0，只会记住这个路径，但不会真正切换到 E: 盘；
只有先切换到目标盘符，再用 cd 切换文件夹，或者用带 /d 参数的 cd 命令，才能一次性跨盘符切换。
正确切换到 E 盘文件夹的两种方法
方法 1：分步切换（先切盘符，再切文件夹）
# 第一步：切换到 E 盘（直接输入盘符+冒号）
E:
# 第二步：切换到 E 盘下的目标文件夹
cd E:\metallographic_analyzer_v3.0
方法 2：一次性跨盘符切换（推荐，更高效）
用 cd /d 命令（/d 参数表示 “drive”，强制跨盘符切换）：
# 无论当前在哪个盘，一次性切换到 E 盘的目标文件夹
cd /d E:\metallographic_analyzer_v3.0

你在 E:\metallographic_analyzer_v3.0 文件夹下执行 git add . 时提示 fatal: not a git,repository，这是因为这个文件夹还没有被初始化为 Git 仓库——Git 只能管理已初始化（创建 .git 目录）的文件夹，我来教你完整的解决步骤。
1. 问题核心原因
not a git repository 翻译过来是 “不是 Git 仓库”，意味着当前文件夹里没有 Git 仓库的核心目录 .git（这
个目录默认隐藏）。你之前操作的 learngit 是已初始化的仓库，但 metallographic_analyzer_v3.0 是全
新的文件夹，需要先初始化。
2. 完整解决步骤（从初始化到添加文件）
你已经在 E:\metallographic_analyzer_v3.0> 提示符下，说明路径正确。
步骤 2：初始化 Git 仓库（关键！）
执行初始化命令，会在当前文件夹创建隐藏的 .git 目录，标志这是一个 Git 仓库：
git init
执行后会看到类似反馈，说明初始化成功：
Initialized empty Git repository in E:/metallographic_analyzer_v3.0/.git/
步骤 4：添加文件 / 文件夹到暂存区
现在再执行 git add . 就可以正常添加当前文件夹下所有文件 / 文件夹了：
git add .
 验证：执行 git status，会看到哪些文件被暂存（比如）
 Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   xxx.py
        new file:   xxx.txt
步骤 5：提交到 Git 仓库
添加到暂存区后，提交才能真正纳入版本管理：
Git 2.35.0 及以上版本新增了 “可疑所有权检测” 功能：如果 Git 仓库文件夹的属主用户，和当前执行 Git 命令的用
户不一致，Git 会认为这个仓库可能有安全风险，从而阻止操作。
你的情况就是：E:\metallographic_analyzer_v3.0 属于管理员组（BUILTIN/Administrators），但你是用普
通用户（MSI/15041）执行 Git 命令，触发了这个安全校验。\
2. 快速解决：添加安全目录例外（推荐）
报错提示里已经给出了解决方案，直接执行这条命令即可（复制粘贴到终端）：
git config --global --add safe.directory E:/metallographic_analyzer_v3.0
如果你不想添加全局例外，也可以临时以管理员身份打开终端：
右键点击 Windows 终端 / CMD/Git Bash；
选择 “以管理员身份运行”；
重新切换到 E:\metallographic_analyzer_v3.0 目录，再执行 git add .。
至此基本掌握git用法