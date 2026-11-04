# JupyterLab 与 the-craft-of-selfteaching 学习流程

这份指南记录如何在本机打开 JupyterLab、阅读和记录学习内容、用 Git 保存版本，并把学习分支上传到 GitHub。命令请在 macOS 的终端中逐条运行；看到错误时先停下来，保留完整错误信息再处理。

## 1. 本机学习环境

截至 2026-11-04，已核对的环境如下：

- JupyterLab 安装在 `~/JupyterLab/.venv`，版本为 4.6.5。
- JupyterLab 的工作区根目录是 `~/JupyterLab`。
- 书籍仓库在 `~/JupyterLab/Teaching-myself-self-study`。
- GitHub 个人仓库为 `xiaolongwang-jpfund/Teaching-myself-self-study`。
- 本地学习分支叫 `learning`，对应 GitHub 上的 `origin/learning`。
- 本次 Fork 页面显示仓库为 Public（公开）；推送到 GitHub 的学习笔记任何人都可能看到。

目录关系：

```text
~/JupyterLab/
├── .venv/                          # JupyterLab 使用的独立 Python 环境
├── jupyter-first-steps.ipynb       # 工作区自己的入门 Notebook
├── JupyterLab-使用说明.md           # 本指南
└── Teaching-myself-self-study/     # Git 仓库：书籍和学习记录
```

`~/JupyterLab` 是放置文件和仓库的工作区，本身不是 Git 仓库。真正的 Git 仓库是里面的 `Teaching-myself-self-study` 子文件夹；在终端中应先进入该子文件夹，再运行 Git 命令。

## 2. 启动和停止 JupyterLab

### 启动

打开终端，依次运行：

```sh
cd ~/JupyterLab
source .venv/bin/activate
jupyter lab
```

- `cd` 进入 JupyterLab 工作区。
- `source .venv/bin/activate` 启用本机的 Python 虚拟环境（项目专用的 Python 与工具目录）。
- `jupyter lab` 启动本机网页应用。启动服务的终端窗口要保持打开。

启动后，浏览器会打开 JupyterLab。左侧文件栏从 `~/JupyterLab` 开始显示，所以可以看到 `Teaching-myself-self-study` 文件夹。双击文件夹进入仓库，再双击想读的 `.ipynb` 文件。

如果 JupyterLab 已经在运行，切回现有的浏览器标签即可；不要重复启动。若浏览器没有自动打开，查看终端显示的本机地址。地址中可能含有 `token=` 访问令牌，不要把完整地址发给别人或贴到公开位置。

### 停止

1. 切回运行 JupyterLab 的终端。
2. 按 `Control-C`。
3. 如果终端询问 `Shut down this Jupyter server (y/[n])?`，输入 `y` 并按 Return。
4. 看到终端提示符重新出现，就表示服务已停止；浏览器页面断开是正常的。

## 3. 阅读与记录学习笔记

### 打开书籍章节

在 JupyterLab 左侧文件栏中进入：

```text
Teaching-myself-self-study/
```

书籍主要章节通常是 `.ipynb` Notebook；例如 Proof of Work 章节是：

```text
02.proof-of-work.ipynb
```

对应的 Markdown 原文在 `markdown/` 子文件夹中。Notebook 可以显示文字、代码和运行结果；Markdown 原文适合直接阅读。

### 在 Notebook 中学习

- Markdown 单元格用于正文和学习笔记；Code 单元格用于 Python 代码。
- 阅读文字时不需要运行代码。需要练习代码时，在 Code 单元格中按 `Shift + Return` 运行。
- 运行结果会显示在代码格下方；`In [1]` 一类编号表示该内核会话中的运行顺序。
- 按 `⌘S` 保存当前 Notebook。JupyterLab 也可能自动保存，但重要修改后仍建议手动保存。
- 若要在章节 Notebook 末尾记一条总结，先选中最后一个单元格，再点工具栏的 `+` 添加单元格；确认单元格类型为 Markdown，再输入笔记并保存。
- 若误加空单元格，先选中并确认它确实为空，再用 **Edit → Delete Cells** 删除，最后保存。

也可以把较长的个人笔记另存为 Markdown 文件，放在仓库的 `my-notes/` 文件夹中。建议使用清楚的英文文件名，例如：

```text
my-notes/02-proof-of-work-notes.md
```

章节 Notebook 适合边读边做的小修改；`my-notes/` 适合整理较完整的个人笔记。两种方式都可以纳入 Git 学习记录。

## 4. Git 版本跟踪：分支、暂存、提交

Git 是版本控制工具。它保存你选定的文件在某个时间点的版本，以及提交说明等记录；它不会自动记录每次按键、全部思考过程或投入程度。提交记录能展示留下来的工作轨迹，但不能单独证明学习者实际理解了所有内容。

### 进入仓库并检查状态

每次开始处理学习记录前，先运行：

```sh
cd ~/JupyterLab/Teaching-myself-self-study
git status --short --branch
```

预期分支是 `learning`。这个分支专门保存个人学习过程；`master` 用作 Fork 中书籍原始版本的基线。个人学习不要提交到 `master`。

本机已创建并推送 `learning` 分支。以后继续学习时运行：

```sh
git switch learning
```

只有第一次创建学习分支时才运行 `git switch -c learning`；已有分支时不要重复创建。

### 检查修改

保存 Notebook 后，先检查有哪些文件发生变化：

```sh
git status --short
git diff
```

`git diff` 查看尚未暂存的修改。`.ipynb` 是结构化文件，差异内容可能包含单元格、内核名称或 Python/Notebook 版本信息。JupyterLab 保存时可能更新这些环境信息；提交前确认差异里有预期的笔记，没有误加空白代码格或无关文件。

### 暂存和提交

暂存（staging）是选定哪些改动进入下一次提交。每次只暂存本次学习相关文件，例如：

```sh
git add 02.proof-of-work.ipynb
```

若新增了 Markdown 笔记，则改用对应路径，例如：

```sh
git add my-notes/02-proof-of-work-notes.md
```

查看暂存区内容：

```sh
git status --short
git diff --cached
```

确认文件和内容正确后，创建本地提交（commit）：

```sh
git commit -m "Add Proof of Work reading note"
```

提交说明尽量简短并写明做了什么。提交只保存到本机 Git 历史，不会自动上传。`git add` 加上明确文件路径，比在不确定当前目录内容时使用 `git add .` 更容易控制范围。新文件需要先 `git add`；单独使用 `git commit -am` 不会把未跟踪的新文件纳入提交。

## 5. 上传到 GitHub

推送（push）会把本地提交上传到 GitHub。此仓库当前的 `learning` 分支已设置追踪 `origin/learning`，后续在该分支提交后运行：

```sh
git push
```

第一次推送一个尚未发布的分支时，使用：

```sh
git push -u origin learning
```

`-u` 会把本地 `learning` 与 GitHub 上的 `origin/learning` 关联起来；以后通常只需 `git push`。

如果 Git 请求授权，按提示使用浏览器或 Git Credential Manager 完成 GitHub 登录；不要把密码、验证码或访问令牌写入 Notebook、笔记文件或公开仓库。

推送后检查：

```sh
git status --short --branch
git log -1 --oneline
```

正常情况下，状态会类似：

```text
## learning...origin/learning
```

这表示本地分支与 GitHub 上的对应分支同步。`git log -1 --oneline` 显示最近一次提交。也可以在 GitHub 个人仓库页面切换到 `learning` 分支，查看提交和文件。

## 6. 一个章节的完整学习循环

1. 启动或切回 JupyterLab，打开书籍章节。
2. 阅读、运行有帮助的代码，或在 Markdown 单元格/`my-notes/` 记录自己的理解。
3. 按 `⌘S` 保存。
4. 在仓库终端运行 `git status --short --branch` 和 `git diff`，检查修改。
5. 用 `git add <文件路径>` 暂存本次学习内容。
6. 用 `git diff --cached` 复核暂存内容。
7. 用 `git commit -m "..."` 留下清晰的本地记录。
8. 用 `git push` 上传到自己的 GitHub Fork。
9. 确认 `git status` 显示本地分支与 `origin/learning` 同步。

不必每读几行就提交。可以在完成一个小节、一次代码练习或一份有意义的笔记后提交，让每条记录有清楚的主题。

## 7. 给原书提交校对建议（可选）

个人学习记录保存在 `learning` 分支。只有发现可复现的错字、代码错误或明确有益的表述改进，并准备让作者审阅时，才走 Pull Request（PR，合并请求）流程。不要把个人笔记或整本学习分支提交给原书仓库。

校对前先把 GitHub 上自己的 `master` Fork 更新到作者仓库最新版本：

1. 打开自己的 Fork 仓库主页。
2. 使用 **Sync fork** 检查上游更新；若页面显示已同步，就不需要操作；若有更新，查看变更后选择更新分支。
3. 在本机切回并更新自己的 `master`，然后从它创建单独的校对分支：

```sh
cd ~/JupyterLab/Teaching-myself-self-study
git switch master
git pull origin master
git switch -c fix/short-description
```

把 `short-description` 换成简短英文主题，例如 `fix/typo-proof-of-work`。从更新后的 `master` 开始，能避免把 `learning` 分支里的个人学习提交混进校对。

4. 只改一个小而明确的问题，检查修改，再暂存并提交：

```sh
git status --short
git diff
git add <修改文件路径>
git diff --cached
git commit -m "Fix a typo in Proof of Work chapter"
git push -u origin fix/short-description
```

5. 打开自己的 GitHub Fork，使用 **Contribute** 或 **Compare & pull request** 创建 PR。确认目标（base）是作者的 `xiaolai/the-craft-of-selfteaching` 仓库及其 `master` 分支；来源（head）是自己的 Fork 和刚推送的 `fix/...` 分支。写清楚修改原因，检查比较结果后再创建 PR。

GitHub 的按钮名称和页面布局可能变化；始终核对 PR 的目标仓库、目标分支和改动文件。PR 是向作者提出修改建议，不代表修改已被接受；作者审阅并合并后，改动才进入原书。

## 8. 常见情况

### `fatal: not a git repository`

当前终端不在书籍仓库目录。运行：

```sh
cd ~/JupyterLab/Teaching-myself-self-study
```

再试 Git 命令。`~/JupyterLab` 是工作区，不是这个书籍仓库本身。

### `nothing to commit`

Git 没找到已暂存的新修改。确认 Notebook 已按 `⌘S` 保存，再检查 `git status --short`。若文件列表里没有修改，可能内容尚未更改，或已提交。

### `git push` 提示没有 upstream

第一次发布当前分支时运行：

```sh
git push -u origin learning
```

### Notebook 的 Git 差异很多

先用 `git diff -- 02.proof-of-work.ipynb` 查看差异。Notebook 会保存单元格和环境元数据；删除意外添加的空白单元格，保留有意的学习内容与合理的内核信息，再保存并复核。

### JupyterLab 没有打开或想停止

启动服务的终端不要关闭。需要停止时按本指南第 2 节操作。地址中的 `token=` 是本机访问保护的一部分，不要公开分享。

## 9. 当前学习记录

截至 2026-11-04，Proof of Work 章节的一条学习总结已提交并推送：

- 分支：`learning`
- 提交：`4cd0c4c Add Proof of Work reading note`
- 记录文件：`02.proof-of-work.ipynb`

这个状态是当时核对的快照；开始新一轮学习时，先运行 `git status --short --branch` 查看当前状态。
