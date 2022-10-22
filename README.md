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
### tbd
### tbd
### tbd
### tbd
### tbd


