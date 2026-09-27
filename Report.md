# 1.回答问题

1. 你之前有过多人协同开发的经历吗？如果有，你们是使用什么方式分工协作的？
- 此前没有正式参与过多人协同开发。过去进行课程作业或编程练习时，主要以个人完成为主。如果多人合作，通常通过文件共享、即时通讯工具等方式交换代码和修改内容。学习 Git 后，我理解到 Git 可以通过分支、提交、合并等机制记录每个人的修改，并减少多人直接修改同一份代码所造成的问题。

2. 思考一下，Git 为什么要设计“暂存-提交”两个步骤？
- Git 设计暂存区，主要是为了让人精确控制下一次提交包含哪些改动。它把“工作区里改了什么”和“这次提交要记录什么”分开了。提交时记录的是暂存区，而不是工作区当前状态。

3. `git branch` 和 `git branch -a` 的区别是什么？查阅资料并回答。
- `git branch` 用于查看本地分支， `git branch -a` 用于查看本地分支与远程跟踪分支。

# 2. 第一次commit
## 2.1
首先以提供的仓库为模版新建仓库`ics-GitLab`，随后在新建的文件夹icsLab中，终端运行
```
git clone git@github.com:fluutter/ics-GitLab.git
```
将GitHub对应位置的仓库克隆到本地git。
## 2.2
随后在本地打开其中的main.c文件并对第五行进行修改。
![[11modifiedNotAdd.png]]
修改完成后，终端输入`git status`，可以发现这里显示main.c文件被更改，但未提交。同时由于运行了c程序产生的`.vscode/, main, main.dSYM/`等文件未被git跟踪。
## 2.3
接下来终端输入`git add main.c`将`main.c`添加至暂存区。
再输入`git status`，可以发现`modified:    main.c`这一行变成绿色，并且显示更改待提交。
![[12Added.png]]
## 2.4
接下来终端输入`git commit -m "Complete TODO in main.c"`，将`main.c`提交。
输入`git log --online`可以看到本地的main版本更新至刚刚更改的"Complete TODO in main.c"，但GitHub上的版本仍为Initial commit。
![[13NotPush.png]]
## 2.5
接下来执行`git push`将本地git更改推到GitHub上。
再输入`git log --online`可以看到，本地git和GitHub的版本都是更新过的版本了。
![[14Pushed.png]]
# 3. 阅读与回答问题
## 3.1
[Commit message 和 Change log 编写指南](https://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)这个文档介绍了：
规范 Commit Message 有助于提高 Git 提交记录的可读性，便于开发者理解代码修改、查阅项目历史，并为自动生成 Change log 提供基础。标准格式主要包括 Header、Body 和 Footer 三部分。其中，Header 为必填项，格式为 `<type>(<scope>): <subject>`，分别表示提交类型、修改范围和简短描述；Body 用于详细说明修改的原因和内容；Footer 用于标注不兼容变更或关联的 Issue。常见的提交类型包括 feat（新增功能）、fix（修复 Bug）、docs（文档修改）、style（格式调整）、refactor（代码重构）、test（测试）和 chore（辅助工作）。
## 3.2 
[Git使用规范](https://notes.vectania.com/article-git-gif-flow)介绍了Gitflow 工作流的基本原理、分支管理方式及具体操作流程。其核心思想是通过不同分支分离开发、测试和发布过程，降低多人协作中的代码冲突和版本管理风险。
文章介绍了五种关键分支：
- master：存放已正式发布的稳定代码，每次发布后打 Tag。
- develop：用于日常开发，作为各功能分支的公共基础。
- feature：从 develop 创建，用于独立开发新功能，完成后合并回 develop。
- release：从 develop 创建，用于版本发布前的测试和缺陷修复，完成后合并到 master 和 develop。
- hotfix：从 master 创建，用于紧急修复线上 Bug，修复后合并回 master 和 develop。
同时，文章还介绍了 Gitflow 的基本命令及版本号规范。版本号采用 `主版本号.次版本号.修复版本号` 的形式，分别对应重大变更、功能迭代和 Bug 修复。总的来说，Gitflow 通过明确分支的职责及合并方向，使代码开发、测试、发布和紧急修复形成相对清晰的流程。
## 3.3 为什么要学习 Git
学习 Git 的价值在于让我能够记录代码的修改历史，更有条理地管理自己的代码与项目，记录学习过程，进行多人协作，并为今后参与更复杂的编程项目打下基础。
# 4. 制造并解决冲突
## 4.1
在文件夹中终端处输入`git branch feature`新建一个名为feature的分支，输入`git switch feature`转到这个新分支。
![[21newBranch.png]]
## 4.2
在feature分支处，修改main.c的第五行代码，并add与commit。
![[22FeatureEdit.png]]
随后，转回main分支，再次修改main.c的第五行代码，并add与commit。
![[23MainEdit.png]]
## 4.3
输入`git merge feature`试图合并两个分支。显示产生冲突，自动合并失败。在无法自动确定同一处修改应如何组合时，需要用户介入解决冲突。
![[24CONFLICT.png]]
## 4.4
点击`<<<<<<< HEAD (Current Change)`上方的“Accept both changes”后，add。
查看git状态，显示冲突已解决。
![[25merged.png]]
## 4.5
接下来commit，随后查看git的提交历史图。
![[26tree.png]]
- `112173a` 是最初克隆下来的版本。
- `cf73c9a` 是 `main` 上第一次修改与提交的版本。
- `e775b91` 是 `main` 分支上的修改。
- `443cef7` 是 `feature` 分支上的修改。
- `d8c8934` 是解决冲突后产生的合并提交。
- 两条分支最终在 `d8c8934` 汇合。