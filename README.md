# Junior-Software-Engineer-s-Notebook 📖
## Git
### 📍 VCSs, Git, Github/Gitlab
### VCSs = Version Control System(VCSs)
### Git = Git is the de facto standard for version control 
### Github/ Gitlab/ Gitee = the host of Git Repository

## 📍 Snapshots 📷
### Git models the history of a collection of files and folders within some top-level directory as a series of snapshots📷.
> ```
> <root> (tree)
> |
> +- foo (tree)
> |  |
> |  + bar.txt (blob, contents = "hello world")
> |
> +- baz.txt (blob, contents = "git is wonderful")
> ```

## 📍 Modeling history: relating snapshots
### a history is a directed acyclic graph(DAG) of snapshots
### each snapshot in Git refers to a set of "parents", the snapshot that preceded it.
> ```
>     this is a commit
>             ↑
> o <-- o <-- o <-- o
>             ^
>              \
>                --- o <-- o
> ```

> ```
>       new_feature       new_feature + bug_fix
>             ↑               ↗
> o <-- o <-- o <-- o <---- o
>             ^
>              \
>                --- o <-- o
>                          ↓
>                       bug_fix
> ```
  
## 📍 Fast-forward and three-way merge 
### Fast-forward: the commit all points to a same parent commit
> ```
> o <-- o <-- o <-- o  
> ```

### Three-way merge:
### - master and sub no conflicts ✔️, different file
### - master and sub no conflicts ✔️, different modification in the same file
### - master and sub conflicts ❌  
  
> ```
> o <-- o <-- o <-- o <---- o
>             ^            /
>              \          v
>               --- o <-- o
> ```

## 📍 Data model 
### The following mimics the data model in Git in pseudocode.
### **File**: it's a bunch of bytes
> ```
> type blob = array<byte>
> ```

### **Directory**: It contains named files and directories //目录
> ```
> type tree = map<string, tree | blob>  
> ```

### **Commit**: It has parents, metadata, and the top-level tree //
> ```
> type commit = struct
> {
>     parents: array<commit>
>     author: string
>     message: string
>     snapshot: tree  
> }  
> ```
  
  
### **Object**: It could be a blob, tree, or commit. //对象
> ```
> type object = blob | tree | commit
> ```
  
### **Data Storage**: In Git data store, all objects are content-addressed by their SHA-1 hash.
> ```
> objects = map<string, object>
> 
> def store(object):
>     id = sha1(object)
>     objects[id] = object
>  
> def load(id):
>     return objects[id]
> ```
  
### **References**: They are pointers to commits, Convert _SHA-1 hash_ to _human-readable names._
> ```
> references = map<string, string>
> def update_reference(name, id):
>     reference[name] = id
>
> def read_reference(name):
>     return references[name]
>
> def load_reference(name_or_id):
>     if name_or_id in references:
>         return load (references[name_or_id])
>     else:
>         return load(name_or_id)
> ```  

e.g.

`HEAD` is the latest "where we currently are"
`master` refers to a particular snapshot instead of a bunch of hexadecimal string.
  
  
## 📍 Repositories
in short, a Git _repository_: it is the data `objects` and `references`.
  
## 📍 A diagram for Git

# Frequently Used Commands //常用命令
|Command 命令 |Objective 目的 |Example 例子 |
|------------|---------------|------------|
|`git add <file_name>`|Add file to staging area 添加📃到暂存区|`git add README.md`|
|`git add .`|Add any unstaged files to staging area 将任何未暂存区的文件添加到暂存区|
|`git add -p <file>`|Interactively choose hunks of patch 交互式选择大块补丁|
|`git blame`|see each commit with authors 查看每个提交与作者|
|`git branch`|see all the branches 查看所有分支|
|`git branch <branch_name>`|create a new branch 创建一个心分支|`git branch dev`|
|`git branch -d <the_local_branch>`|Delete local branch 删除本地分支|`git branch -d PointDebug`|
|`git cat-file -p <SHA-1 hash>`|Visualize data by SHA-1 hash 可视化数据|
|`git checkout <file>`|remove the unstaged changes and back to current stage 删除未暂存的更改并返回到当前阶段｜
|`git checkout <branch>`|change HEAD to such branch|`git checkout dev`|
|`git checkout <commit_guid>`|change HEAD to such commit|
|`git checkout -b <branch>`|create a new branch and checkout to it 创建一个新分支并checkout 它 `git checkout -b dev`|
|`git clean -f -d`|clean all the untracked files and folders 清理所有未跟踪的📃和📁｜
|`git clone <url>`|clone the repo from url 从url clone repo｜
|`git clone --shallow`|clone the repo without any history clone没有任何历史记录的存储库|
|`git clone --recursive <url>`｜clone the repo recursively 以递归方式clone存储库｜
|`git commit`|`Archive and confirm` the changes to the directory 存档并确认对目录的修改|
|`git commit -m"<message>"`|same with 👆,but with a short message|`git commit -m"UpdateREADME.md"`|
|`git commit -a-m"<message>"`|`-a` stands for add; combine add and commit 合并添加和提交|`git commit -a -m"Update README.md"`|
|`git commit --amend`|append current commit with previous commit 追加当前提交到上一个提交|
|`git config -l`| `-l` stands for listing the details of config, e.g. user name 列出config的细节|
|`git diff`|
|`git diff --staged`|compare the un-commit changes with HEAD|
|`git diff <file>`|see diff of file with HEAD|
|`git diff <guid> <file>`|see diff of file between current HEAD and guid|
|`git diff <prev_guid> <now_guid> <file>`|see diff of file between `previous` and `now`|
|`git fetch <remote>`|fetch the commits from remote to local 从远程取得commits到本地|
|`git help <command>`|help menu of such command|`git help commit`|
|`git init`|Initialize a git repo 初始化一个git repo|
|`git log`|the log of git history git历史日志|
|`git log --stat`|the log of git history statistically git历史日志|
|`git log --all --graph --decorate`|nice diagram of git log git日志图解|
|`git merge <branchX>`|merge `branchX` into current branch|`git merge origin/master`|
|`git mv <old_name> <new_name>`|rename specific file(this has to be commited)|
|`git push`|push commits to remote(already configured remote)|
|`git push <remote> <branch>:<branch>`|push commits to remote|git push origin main:main|
|`git push <remote> --delete <the_remote_branch>`|Delete remote branch 删除远程分支|git push origin --delete PointDebug|
|`git pull`|`git pull`={`git fetch; git merge<remote>/<branch>`}|
|`git rm --cached <file>`|To stop tracking files which have already been tracked|`git rm --cached main.3dm.bak`|
|`git reset`|remove all the staged changes, green=>red|
|`git reset --hard <commitGuid>`|Destroy any local modification and reset to such commit|`git reset --hard 0d1d7fc32`|
|`git reset HEAD --hard`|Reset the modified files back to the HEAD|
|`git revert <commit_guid>`|Reverting undoes a commit by creating a new commit|
|`git show <guid>`|check specific commit by guid|`git show 721d6bd`|
|`git stash`|hid current untracked changes|
|`git stash pop`|pop out the hidden untracked changes|

# 2. Good 👍 resources of Git
#### `.gitignore` template
> https://github.com/github/gitignore

#### Software for Git //Git 软件
> `SourceTree` is a free software managing Git while it provides GUI to interact with Git. Highly recommend!
> https://www.sourcetreeapp.com/

#### Book for Git //Git
> https://git-scm.com/book/en/v2

# 3. Some Regular Procedure 一些常规流程
### 📍 How to solve conflicts? //如何解决冲突
#### Suppose you are on `master`, and you want to merge `new_feature` branch, the conflict is on `main.cpp`. then you can do:
> ```
> git merge new_feature  
> ```
  
#### You will see the feedback from the prompt saying there are conflicts. You open the `main.cpp` which was decorated with some notation from `git`. Here I use VS Code
> ```
> code main.cpp  
> ```

#### You manually fix the conflicts, Then you should **add it to the staging area!** 
> ```
> git merge --continue 
> ```
  
### 📍 Setup for recursive clone? //递归克隆
#### You want to combine several dependencies into one project when you work on a macro project //合依赖到一个大型project
1. create a `.gitmodules` in the root of your `git` folder.
2. define **what** `git` repo will be in which **location** of your main folder
> ```
> [submodule "<name_of_this_repo"]
>         path = <location_of_this_submodule>
>         url = <url_of_this_repo>
> //e.g.
> [submodule "deps/polyscope"]
>         path = deps/polyscope
>         url = https://github.com/nzfeng/polyscope.git
> [submodule "deps/googletest"]
>         path = deps/googletest
>         url = https://github.com/google/googletest.git
> ```

### 📍 Git submodules
> ```
> git submodule add --depth 1 git@gitee.com:shanghai-dajie-robot/pocket_raichu.git submodules/pocket_raichu
> git config -f .gitmodules submodule.pocket_raichu.shallow true  
> ```

> ```
> git config -f .gitmodules submodule.submodules/pocket_raichu.shallow false  
> ```
  
#### git submodule foreach git pull origin master // 每个git拉取原始master
> ```
> git clone <repo_url>
> git submodule init
> git submodule update --depth 10  
> ```
  
### 📍 Setup a Github Access Token
1. Go to the Github account - Developer Setting - Generate Token
2. Git clone an arbitrary repo from your page
3. When the computer request credential, just close it until it appears on the command line for the following info //当💻请求凭据，关闭直到在CL出现下述信息
-  i. `user_name`: the name of your Github account
-  ii. `password`: paste your token here
  

# Shell 🐚
> in short, **shell** is a type of user interface. It is either command-line interface (CLI) or **graphical user interface(GUI)** this section will mainly on_(CLI)_ especially on Bourne Again SHell, or "bash" for short.
> 

## 0.Basic Concept //基本概念
### 📍environment variable(PATH)
All the commands you run in the shell has already been added to the `environment variable` in your PC. In Windows, you can search `edit the system environtment variable` => `environtment variable..` =>Edit the `PATH`.
  
### 📍`/` anf `\`
### A path on the shell is a delimited list of directories;
### on Linux and macOS
> path separated by `/`
> `/` means the "root"
  
### on Windows:
> path separated by `\`
> > `C:\` means the "root"

### 📍 absolute path and relative path //绝对路径与相对路径
> a.Absolute path is with full path (literally). e.g.
> > In windows,
> ```
> C:\Users\Chenxing\AppData\Local\Temp
> ```

> > In Linux and macOS:
> > > path starts with `/` is _absolute_
> ```
> /home/tutorial   
> ```

> b. Relative path takes the _advantage_ of _environtment variable_ to form the absolute path. e.g.
> ```
> %USERPROFILE%\AppData\Local\Temp
> ```

### 📍 `.` , `..` , `~` , `-` used in path
### It is a must to know these 3 symbols for they appear a lot.
### `.` means current directory, e.g. `cd ./tutorial` change dir to the tutorial folder which under **current dir**
### `..` means the parent directory, e.g. `cd ..` change dir to its parent dir. 
### `~` means the home directory. e.g. `ssh-add ~/.ssh/id_rsa` add the ssh key in the "HOME folder/.ssh/"
### `-` means the previous directory in the prompt

### 📍 `-`, `--` used in command
### `-` indicates a flag which **modify** their behavior
### `--` indicates an options
### _example:_
> ```
> ls -l  
> ```
### 
