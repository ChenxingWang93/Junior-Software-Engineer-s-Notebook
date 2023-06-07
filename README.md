# Junior-Software-Engineer-s-Notebook 📖  //初级软件工程师


## Git
### 📍 VCSs, Git, Github, Gitlab, Gitee //
### VCSs = Version Control System(VCSs) //版本控制系统
### Git = Git is the de facto standard for version control //版本控制的 de facto 标准
### Github/ Gitlab/ Gitee = the host of Git Repository //Git 存放处的宿主

## 📍 Snapshots //快照📷
### Git models the history of a collection of files and folders within some top-level directory as a series of snapshots📷. //Git模型，以顶层目录做为系列快照文件📃与📁集的历史
> ```
> <root> (tree)
> |
> +- foo (tree)
> |  |
> |  + bar.txt (blob, contents = "hello world")
> |
> +- baz.txt (blob, contents = "git is wonderful")
> ```

## 📍 Modeling history: relating snapshots //建模历史：相关快照
### a history is a directed acyclic graph(DAG) of snapshots // 
### each snapshot in Git refers to a set of "parents", the snapshot that preceded it. //Git 中的每一个 快照 
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
  
## 📍 Fast-forward and three-way merge // 快进 ⏩ 与 三路 merge 合并
### Fast-forward: the commit all points to a same parent commit
> ```
> o <-- o <-- o <-- o  
> ```

### Three-way merge:  //三路 merge 合并
### - master and sub no conflicts ✔️, different file //主、分支 没有冲突，不同文件
### - master and sub no conflicts ✔️, different modification in the same file //主、分支 没有冲突，在相同文件中有不同修改
### - master and sub conflicts ❌  //主、分支 冲突
  
> ```
> o <-- o <-- o <-- o <---- o
>             ^            /
>              \          v
>               --- o <-- o
> ```

## 📍 Data model //数据模型
### The following mimics the data model in Git in pseudocode. //下列用 用伪🥸代码 模仿了 Git 中的数据模型 
### **File**: it's a bunch of bytes //一系列的bytes
> ```
> type blob = array<byte>
> ```

### **Directory**: It contains named files and directories //目录：包含名为 文件📃、目录
> ```
> type tree = map<string, tree | blob>  
> ```

### **Commit**: It has parents, metadata, and the top-level tree //parents，元数据，与顶层树🌲
> ```
> type commit = struct
> {
>     parents: array<commit>
>     author: string
>     message: string
>     snapshot: tree  
> }  
> ```
  
  
### **Object**: It could be a blob, tree, or commit. //对象，树，或者 commit
> ```
> type object = blob | tree | commit
> ```
  
### **Data Storage**: In Git data store, all objects are content-addressed by their SHA-1 hash. //数据存储：在Git 数据存储中，所有的对象都是 通过 SHA-1 hash 来做内容寻址 
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
  
### **References**: They are pointers to commits, Convert _SHA-1 hash_ to _human-readable names._ //参考 
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
 
  
## 📍 Repositories  //仓库
in short, a Git _repository_: it is the data `objects` and `references`.
  
## 📍 A diagram for Git //Git 图解

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

# 2. Good 👍 resources of Git //Git 资源
#### `.gitignore` template
> https://github.com/github/gitignore

#### Software for Git //Git 软件
> `SourceTree` is a free software managing Git while it provides GUI to interact with Git. Highly recommend!
> https://www.sourcetreeapp.com/

#### Book for Git //Git 书籍
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

### 📍 `.` , `..` , `~` , `-` used in path //在路径中
### It is a must to know these 3 symbols for they appear a lot. //必须要了解的3个符号，因为他们出现了很多次
### `.` means current directory, e.g. `cd ./tutorial` change dir to the tutorial folder which under **current dir** //
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
### The `-l` flag asks the ls command to list the files in _a long listing format_
### _example:_
> ```
> git log --stat  
> ```
### The `--stat` option indicates the `git` command to show the _statistics_ from `git log`

### 📍 ` `, `""`, `\` **use in file name and folder name**
### The space ` ` is used to seperate the arguments in the command line. e.g.
> ```
> mkdir my photo  
> ```

### This will create 2 folders which are "my" and "photo". If you want to create a folder with "my photo".
### You either use `""` to concatenate or use `\` to escape the space.
> ```
> mkdir "my photo"
> mkdir my\ photo
> ```

### 📍 `' '` and `" "`
### Things inside single quote `' '` are literal string. Meaning what is inside.
### Things inside double quote `" "` are strings. They can be substituted.

### 📍 `<`, `>`, `>>` in data stream
### `<` take the data stream out...
### `>` take the data stream in...
### `>>` take the data stream _append_ in...
### e.g.
> ```
> cat <hello.txt >README.txt  
> ```

### It means stream out whatever inside `hello.txt` and stream in `README.txt`
> ```
> cat <hello.txt >>README.txt
> ```

### it means stream out content of `hello.txt` and append them at the end of `README.txt`
### 📍 `|`pipe
### It means take the output from the left as the input of the right <A=output> | <A'=output>
e.g.
> ```
> ls -l | tail -n3
> ```
### `ls -l` prints out all the files. This will be treated as the input of `tail` //打印所有文件，当作是`tail`的输入
### `tail -n3` receives the print by `ls -l` and filter the last 3 lines


### 📍 `r`, `w`, `x` file/folder permissions //📃/ 📁权限
### `r`, read 读取
> file: read
> dir: allow? to see files in this dir

### `w`, write 写入
> file: write
> dir: allow? to rename/remove files in this dir

### `x`, execute 执行
> file: execute
> dir: allow? to enter this dir

### `d`, directory 目录
### `-`, nope
### in the beginning, there are **10 characters**.

### `0`:indicates if this a directory. `d`:directory; `-`:a file
### `1 to 3`: the permission of **file owner**
### `4 to 6`: the permission of **the owner group**
### `7 to 9`: the permission of **other users**
e.g.
> ```
> drwxr-xr-x 1 Chenxing 197121      0 Aug  5 01:15  Autodesk/
> -rwxr-xr-x 1 Chenxing 197121   2475 Jul 28 20:01 'Unreal Engine.lnk'*
> -rw-r--r-- 1 Chenxing 197121    282 Jul  1 01:43  desktop.ini
> ```

### For `Autodesk/` folder, `drwxr-xr-x`
> `0=d`, this is a directory✔️, file❌
  
> `123=rwx`, _owner_ can **read**✔️, **write**✔️ and **execute**✔️.
  
> `456=r-x`, _owner group_ can **read**✔️, **write**❌ and **execute**✔️.
  
> `789=r-x`, _other users_ can **read**, **write**❌ and **execute**✔️.
  
For `desktop.ini` file, `-rwxr-xr-x`
> `0=-`, this is a directory❌, file✔️
  
> `123=rw-`, _owner_ can **read**✔️, **write**✔️ and **execute**❌.
  
> `456=r--`, _owner group_ can **read**✔️, **write**❌ and **execute**❌.
  
> `789=r--`, other users can **read**✔️, **write**❌ and **execute**❌.
  
### 📍 `$`, `#` `sudo` in prompt
`$` indicates this is a _normal_ user permission to this shell //shell 的普通用户权限
`#` indicates this is a **super** user permission to this shell //shell 的超级用户权限
`sudo` means **super** user **do** //su 超级，do 权限
> running `echo` in _normal_ user mode 
> ```
> $ echo hello
> ```
  
> ```
> $ sudo apt-get update
> ```

> enter super user mode(in this mode, you don't need to add `sudo` anymore) `su` means shift user

> ```
> $ sudo su
> # echo "I am in the super user mode"
> # exit  
> ```

### 📍 Wildcards matching by `?` and `*`
### `?` substitute with 1 character.
### `*` substitute with **following** characters.
e.g. Suppose you have `main.py main1.py main2.py main3.py`
> ```
> $ ls main?.py
> main1.py main2.py main3.py #It will substitute anything in `?` position 
> $ ls ma*
> main.py main1.py main2.py main3.py #It will match all the characters after *  
> ```

### 📍 Use `{ }` as a set 
### 💡(It works very similar to the philosopy of list matching in Grasshopper.) //
### ⭐ You can see it as the command will iterate what is inside `{ }`
> ```
> $ mkdir {dev,src,master} #create 3 folder at 1 time
> 
> $ mv *{.py,.sh} folder #will move all *.py and *.sh files
>  
> $ convert image.{png,jpg} #This is equal to `convert image.png image.jpg`
> 
> $ touch main{1..28}.py #It will create main1.py, main2.py... all the way to main28.py  
> ```
  

### 📍 Use `find` to do recursive search //使用 `find` 做递归搜索🔍
> 1.Find folders in current dir
- `.` means current dir
- `d` means the search target is directory 
> ```
> $ find . -name dev -type d  
> ```
  
> 2.Find files in depth!! //深度寻找📃
- `**/bin/*.dll` means no matter what is the front, the most important pattern is `**/bin*.dll`
- `f` means the search target is file 
> ```
> $ find . -path '**/bin/*.dll' -type f
> ```

> 3. Find the files been modified //寻找被编辑过的文件📃
- `-mtime` means modified time 
- `-1` means last day 
> ```
> $ find . -mtime  
> ```

> 4. Find the files and delete them //寻找文件并删除
- `*.tmp` all the temporary files
- `-exec rm`execute them with remove command 
> ```
> $ find . -name "*.tmp" -exec rm {} \;
> ```

> 5. Find files by sizes //根据文件📃大小寻找
> ```
> $ find . -size +500k -size -10M -name '*.tar.gz' # Find all zip files with size in range 500k to 10M
> ```

### 📍What is shebang?
> Different names: //不同名
#### It is also called `hashbang, pound-bang, or hash-pling.`
#### Always starts with `#!` at the beginning of a file.
> Objective: //目标
#### Increase the portability of the script. It will make use of the `PATH` environtment and resolve to wherever the command lives in the system. //提升脚本的便携性能，充分利用`PATH`环境 找到系统中存在的 “命令”

> Example: //例子
#### `#!/usr/bin/env python3`, Execute with a Python interpreter, using the `env` program search path to find it. //执行python 编译器，使用 `env` 程序搜索🔍路径

### 📍Arguments //
#### As you see the above case, there are other definitions of arguments in bash.
- `$0` - Name of the script //脚本名
- `$1` to `$9` - Arguments to the script. `$1` is the first argument and so on  //Arguments 到script 
- `$@` - All the arguments  //所有arguments
- `$#` - Number of arguments  //arguments的数量
- `$?` - Return code of the previous command. Similar to `return 0` in C++  //
- `$$` - Process identification number (PID) for the current script //当前脚本的 识别码
- `!!` - Entire last command, including arguments. A common pattern is to execute a command only for it to fail due to missing permission; you can quickly re-execute the command with sudo by doing `sudo !!`  //输入上一个指令，包括arguments
- `$_` - Last argument from the last command. If you are in an interactive shell, you can also quickly get this value by typing `Esc` followed by `.` //
  
### suppose you are at `~`
> ```
> $ mkdir /mnt/new  #the prompt will say you don't have permission
> $ sudo !!  #Here means `sudo mkdir /mnt/new`  
> ```

### Frequently Used Commands //常用命令
|Command  命令|Objective  目标|Example  例子|
|---------|-----------|---------|
|`cat`    |catch whatever inside  捕捉|`cat hello.txt`|
|`cd`     |change directory 改变目录|
|`cp`     |copy a file  复制一个文件|
|`echo`   |like "echo", it simply prints out its arguments  打印出arguments|
|`find`   |find <folder> -name <name> -type <type>|`find . -name main -type f`|
|`fd`     |shortcut for find(not installed yet) find的快捷键|`fd "*py"`|
|`history`|list the history of your typed bash commands bash 命令历史|
|`ls`     |list all the files in current directory  罗列当前目录下的所有文件📃|
|`man`    |manual of something  目录|`man rm` |
|`mkdir`  |make a directory/folder  创建一个目录/文件📃|
|`mv`     |rename/move a file 重命名/移动一个文件|`mv xx.md yy.md`|
|`pwd`    |present working directory 展示工作路径|
|`rm`     |remove a file  移除文件|
|`rm -r`  |remove all the files recursively 递归地移除所有文件|
|`rm -rf *` |remove all the files at the current folder 在现有的文件夹📁中移除所有的文件📃|
|`rmdir`  |remove EMPTY folder|`rm ./.vscode`|
|`rg`     |R.I.P |`rg "import" -t py ~/dev`|
|`tail`   |print the last _n_ lines 打印最后n行  |tail -n3|
|`touch`  |create a file 创建一个文件📃 ｜touch main.cpp |
|`shellcheck`|Debug bash file |`shellcheck mcd.sh`  |
|`<command> --help`|see the function of this command 命令|`git --help`
|`man <command>`|open the menu of this command 打开命令的菜单|`man ls`|
|Ctrl+L|clean out the shell|
  
  
# Acknowledgment 致谢
shout out to my former colleague XingxinHe [Junior-SofwareEngineer-Notes](https://github.com/XingxinHE/Junior-SofwareEngineer-Notes)
