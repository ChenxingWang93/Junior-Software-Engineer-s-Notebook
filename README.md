# Junior-Software-Engineer-s-Notebook ð  //åçº§è½¯ä»¶å·¥ç¨å¸


## Git
### ð VCSs, Git, Github, Gitlab, Gitee //
### VCSs = Version Control System(VCSs) //çæ¬æ§å¶ç³»ç»
### Git = Git is the de facto standard for version control //çæ¬æ§å¶ç de facto æ å
### Github/ Gitlab/ Gitee = the host of Git Repository //Git å­æ¾å¤çå®¿ä¸»

## ð Snapshots //å¿«ç§ð·
### Git models the history of a collection of files and folders within some top-level directory as a series of snapshotsð·. //Gitæ¨¡åï¼ä»¥é¡¶å±ç®å½åä¸ºç³»åå¿«ç§æä»¶ðä¸ðéçåå²
> ```
> <root> (tree)
> |
> +- foo (tree)
> |  |
> |  + bar.txt (blob, contents = "hello world")
> |
> +- baz.txt (blob, contents = "git is wonderful")
> ```

## ð Modeling history: relating snapshots //å»ºæ¨¡åå²ï¼ç¸å³å¿«ç§
### a history is a directed acyclic graph(DAG) of snapshots // 
### each snapshot in Git refers to a set of "parents", the snapshot that preceded it. //Git ä¸­çæ¯ä¸ä¸ª å¿«ç§ 
> ```
>     this is a commit
>             â
> o <-- o <-- o <-- o
>             ^
>              \
>                --- o <-- o
> ```

> ```
>       new_feature       new_feature + bug_fix
>             â               â
> o <-- o <-- o <-- o <---- o
>             ^
>              \
>                --- o <-- o
>                          â
>                       bug_fix
> ```
  
## ð Fast-forward and three-way merge // å¿«è¿ â© ä¸ ä¸è·¯ merge åå¹¶
### Fast-forward: the commit all points to a same parent commit
> ```
> o <-- o <-- o <-- o  
> ```

### Three-way merge:  //ä¸è·¯ merge åå¹¶
### - master and sub no conflicts âï¸, different file //ä¸»ãåæ¯ æ²¡æå²çªï¼ä¸åæä»¶
### - master and sub no conflicts âï¸, different modification in the same file //ä¸»ãåæ¯ æ²¡æå²çªï¼å¨ç¸åæä»¶ä¸­æä¸åä¿®æ¹
### - master and sub conflicts â  //ä¸»ãåæ¯ å²çª
  
> ```
> o <-- o <-- o <-- o <---- o
>             ^            /
>              \          v
>               --- o <-- o
> ```

## ð Data model //æ°æ®æ¨¡å
### The following mimics the data model in Git in pseudocode. //ä¸åç¨ ç¨ä¼ªð¥¸ä»£ç  æ¨¡ä»¿äº Git ä¸­çæ°æ®æ¨¡å 
### **File**: it's a bunch of bytes //ä¸ç³»åçbytes
> ```
> type blob = array<byte>
> ```

### **Directory**: It contains named files and directories //ç®å½ï¼åå«åä¸º æä»¶ðãç®å½
> ```
> type tree = map<string, tree | blob>  
> ```

### **Commit**: It has parents, metadata, and the top-level tree //parentsï¼åæ°æ®ï¼ä¸é¡¶å±æ ð²
> ```
> type commit = struct
> {
>     parents: array<commit>
>     author: string
>     message: string
>     snapshot: tree  
> }  
> ```
  
  
### **Object**: It could be a blob, tree, or commit. //å¯¹è±¡ï¼æ ï¼æè commit
> ```
> type object = blob | tree | commit
> ```
  
### **Data Storage**: In Git data store, all objects are content-addressed by their SHA-1 hash. //æ°æ®å­å¨ï¼å¨Git æ°æ®å­å¨ä¸­ï¼ææçå¯¹è±¡é½æ¯ éè¿ SHA-1 hash æ¥ååå®¹å¯»å 
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
  
### **References**: They are pointers to commits, Convert _SHA-1 hash_ to _human-readable names._ //åè 
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
 
  
## ð Repositories  //ä»åº
in short, a Git _repository_: it is the data `objects` and `references`.
  
## ð A diagram for Git //Git å¾è§£

# Frequently Used Commands //å¸¸ç¨å½ä»¤
|Command å½ä»¤ |Objective ç®ç |Example ä¾å­ |
|------------|---------------|------------|
|`git add <file_name>`|Add file to staging area æ·»å ðå°æå­åº|`git add README.md`|
|`git add .`|Add any unstaged files to staging area å°ä»»ä½æªæå­åºçæä»¶æ·»å å°æå­åº|
|`git add -p <file>`|Interactively choose hunks of patch äº¤äºå¼éæ©å¤§åè¡¥ä¸|
|`git blame`|see each commit with authors æ¥çæ¯ä¸ªæäº¤ä¸ä½è|
|`git branch`|see all the branches æ¥çææåæ¯|
|`git branch <branch_name>`|create a new branch åå»ºä¸ä¸ªå¿åæ¯|`git branch dev`|
|`git branch -d <the_local_branch>`|Delete local branch å é¤æ¬å°åæ¯|`git branch -d PointDebug`|
|`git cat-file -p <SHA-1 hash>`|Visualize data by SHA-1 hash å¯è§åæ°æ®|
|`git checkout <file>`|remove the unstaged changes and back to current stage å é¤æªæå­çæ´æ¹å¹¶è¿åå°å½åé¶æ®µï½
|`git checkout <branch>`|change HEAD to such branch|`git checkout dev`|
|`git checkout <commit_guid>`|change HEAD to such commit|
|`git checkout -b <branch>`|create a new branch and checkout to it åå»ºä¸ä¸ªæ°åæ¯å¹¶checkout å® `git checkout -b dev`|
|`git clean -f -d`|clean all the untracked files and folders æ¸çæææªè·è¸ªçðåðï½
|`git clone <url>`|clone the repo from url ä»url clone repoï½
|`git clone --shallow`|clone the repo without any history cloneæ²¡æä»»ä½åå²è®°å½çå­å¨åº|
|`git clone --recursive <url>`ï½clone the repo recursively ä»¥éå½æ¹å¼cloneå­å¨åºï½
|`git commit`|`Archive and confirm` the changes to the directory å­æ¡£å¹¶ç¡®è®¤å¯¹ç®å½çä¿®æ¹|
|`git commit -m"<message>"`|same with ð,but with a short message|`git commit -m"UpdateREADME.md"`|
|`git commit -a-m"<message>"`|`-a` stands for add; combine add and commit åå¹¶æ·»å åæäº¤|`git commit -a -m"Update README.md"`|
|`git commit --amend`|append current commit with previous commit è¿½å å½åæäº¤å°ä¸ä¸ä¸ªæäº¤|
|`git config -l`| `-l` stands for listing the details of config, e.g. user name ååºconfigçç»è|
|`git diff`|
|`git diff --staged`|compare the un-commit changes with HEAD|
|`git diff <file>`|see diff of file with HEAD|
|`git diff <guid> <file>`|see diff of file between current HEAD and guid|
|`git diff <prev_guid> <now_guid> <file>`|see diff of file between `previous` and `now`|
|`git fetch <remote>`|fetch the commits from remote to local ä»è¿ç¨åå¾commitså°æ¬å°|
|`git help <command>`|help menu of such command|`git help commit`|
|`git init`|Initialize a git repo åå§åä¸ä¸ªgit repo|
|`git log`|the log of git history gitåå²æ¥å¿|
|`git log --stat`|the log of git history statistically gitåå²æ¥å¿|
|`git log --all --graph --decorate`|nice diagram of git log gitæ¥å¿å¾è§£|
|`git merge <branchX>`|merge `branchX` into current branch|`git merge origin/master`|
|`git mv <old_name> <new_name>`|rename specific file(this has to be commited)|
|`git push`|push commits to remote(already configured remote)|
|`git push <remote> <branch>:<branch>`|push commits to remote|git push origin main:main|
|`git push <remote> --delete <the_remote_branch>`|Delete remote branch å é¤è¿ç¨åæ¯|git push origin --delete PointDebug|
|`git pull`|`git pull`={`git fetch; git merge<remote>/<branch>`}|
|`git rm --cached <file>`|To stop tracking files which have already been tracked|`git rm --cached main.3dm.bak`|
|`git reset`|remove all the staged changes, green=>red|
|`git reset --hard <commitGuid>`|Destroy any local modification and reset to such commit|`git reset --hard 0d1d7fc32`|
|`git reset HEAD --hard`|Reset the modified files back to the HEAD|
|`git revert <commit_guid>`|Reverting undoes a commit by creating a new commit|
|`git show <guid>`|check specific commit by guid|`git show 721d6bd`|
|`git stash`|hid current untracked changes|
|`git stash pop`|pop out the hidden untracked changes|

# 2. Good ð resources of Git //Git èµæº
#### `.gitignore` template
> https://github.com/github/gitignore

#### Software for Git //Git è½¯ä»¶
> `SourceTree` is a free software managing Git while it provides GUI to interact with Git. Highly recommend!
> https://www.sourcetreeapp.com/

#### Book for Git //Git ä¹¦ç±
> https://git-scm.com/book/en/v2

# 3. Some Regular Procedure ä¸äºå¸¸è§æµç¨
### ð How to solve conflicts? //å¦ä½è§£å³å²çª
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
  
### ð Setup for recursive clone? //éå½åé
#### You want to combine several dependencies into one project when you work on a macro project //åä¾èµå°ä¸ä¸ªå¤§åproject
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

### ð Git submodules
> ```
> git submodule add --depth 1 git@gitee.com:shanghai-dajie-robot/pocket_raichu.git submodules/pocket_raichu
> git config -f .gitmodules submodule.pocket_raichu.shallow true  
> ```

> ```
> git config -f .gitmodules submodule.submodules/pocket_raichu.shallow false  
> ```
  
#### git submodule foreach git pull origin master // æ¯ä¸ªgitæååå§master
> ```
> git clone <repo_url>
> git submodule init
> git submodule update --depth 10  
> ```
  
### ð Setup a Github Access Token
1. Go to the Github account - Developer Setting - Generate Token
2. Git clone an arbitrary repo from your page
3. When the computer request credential, just close it until it appears on the command line for the following info //å½ð»è¯·æ±å­æ®ï¼å³é­ç´å°å¨CLåºç°ä¸è¿°ä¿¡æ¯
-  i. `user_name`: the name of your Github account
-  ii. `password`: paste your token here
  

# Shell ð
> in short, **shell** is a type of user interface. It is either command-line interface (CLI) or **graphical user interface(GUI)** this section will mainly on_(CLI)_ especially on Bourne Again SHell, or "bash" for short. 
> 

## 0.Basic Concept //åºæ¬æ¦å¿µ
### ðenvironment variable(PATH)
All the commands you run in the shell has already been added to the `environment variable` in your PC. In Windows, you can search `edit the system environtment variable` => `environtment variable..` =>Edit the `PATH`.
  
### ð`/` anf `\`
### A path on the shell is a delimited list of directories;
### on Linux and macOS
> path separated by `/`
> `/` means the "root"
  
### on Windows:
> path separated by `\`
> > `C:\` means the "root"

### ð absolute path and relative path //ç»å¯¹è·¯å¾ä¸ç¸å¯¹è·¯å¾
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

### ð `.` , `..` , `~` , `-` used in path //å¨è·¯å¾ä¸­
### It is a must to know these 3 symbols for they appear a lot. //å¿é¡»è¦äºè§£ç3ä¸ªç¬¦å·ï¼å ä¸ºä»ä»¬åºç°äºå¾å¤æ¬¡
### `.` means current directory, e.g. `cd ./tutorial` change dir to the tutorial folder which under **current dir** //
### `..` means the parent directory, e.g. `cd ..` change dir to its parent dir. 
### `~` means the home directory. e.g. `ssh-add ~/.ssh/id_rsa` add the ssh key in the "HOME folder/.ssh/"
### `-` means the previous directory in the prompt

### ð `-`, `--` used in command
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

### ð ` `, `""`, `\` **use in file name and folder name**
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

### ð `' '` and `" "`
### Things inside single quote `' '` are literal string. Meaning what is inside.
### Things inside double quote `" "` are strings. They can be substituted.

### ð `<`, `>`, `>>` in data stream
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
### ð `|`pipe
### It means take the output from the left as the input of the right <A=output> | <A'=output>
e.g.
> ```
> ls -l | tail -n3
> ```
### `ls -l` prints out all the files. This will be treated as the input of `tail` //æå°æææä»¶ï¼å½ä½æ¯`tail`çè¾å¥
### `tail -n3` receives the print by `ls -l` and filter the last 3 lines


### ð `r`, `w`, `x` file/folder permissions //ð/ ðæé
### `r`, read è¯»å
> file: read
> dir: allow? to see files in this dir

### `w`, write åå¥
> file: write
> dir: allow? to rename/remove files in this dir

### `x`, execute æ§è¡
> file: execute
> dir: allow? to enter this dir

### `d`, directory ç®å½
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
> `0=d`, this is a directoryâï¸, fileâ
  
> `123=rwx`, _owner_ can **read**âï¸, **write**âï¸ and **execute**âï¸.
  
> `456=r-x`, _owner group_ can **read**âï¸, **write**â and **execute**âï¸.
  
> `789=r-x`, _other users_ can **read**, **write**â and **execute**âï¸.
  
For `desktop.ini` file, `-rwxr-xr-x`
> `0=-`, this is a directoryâ, fileâï¸
  
> `123=rw-`, _owner_ can **read**âï¸, **write**âï¸ and **execute**â.
  
> `456=r--`, _owner group_ can **read**âï¸, **write**â and **execute**â.
  
> `789=r--`, other users can **read**âï¸, **write**â and **execute**â.
  
### ð `$`, `#` `sudo` in prompt
`$` indicates this is a _normal_ user permission to this shell //shell çæ®éç¨æ·æé
`#` indicates this is a **super** user permission to this shell //shell çè¶çº§ç¨æ·æé
`sudo` means **super** user **do** //su è¶çº§ï¼do æé
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

### ð Wildcards matching by `?` and `*`
### `?` substitute with 1 character.
### `*` substitute with **following** characters.
e.g. Suppose you have `main.py main1.py main2.py main3.py`
> ```
> $ ls main?.py
> main1.py main2.py main3.py #It will substitute anything in `?` position 
> $ ls ma*
> main.py main1.py main2.py main3.py #It will match all the characters after *  
> ```

### ð Use `{ }` as a set 
### ð¡(It works very similar to the philosopy of list matching in Grasshopper.) //
### â­ You can see it as the command will iterate what is inside `{ }`
> ```
> $ mkdir {dev,src,master} #create 3 folder at 1 time
> 
> $ mv *{.py,.sh} folder #will move all *.py and *.sh files
>  
> $ convert image.{png,jpg} #This is equal to `convert image.png image.jpg`
> 
> $ touch main{1..28}.py #It will create main1.py, main2.py... all the way to main28.py  
> ```
  

### ð Use `find` to do recursive search //ä½¿ç¨ `find` åéå½æç´¢ð
> 1.Find folders in current dir
- `.` means current dir
- `d` means the search target is directory 
> ```
> $ find . -name dev -type d  
> ```
  
> 2.Find files in depth!! //æ·±åº¦å¯»æ¾ð
- `**/bin/*.dll` means no matter what is the front, the most important pattern is `**/bin*.dll`
- `f` means the search target is file 
> ```
> $ find . -path '**/bin/*.dll' -type f
> ```

> 3. Find the files been modified //å¯»æ¾è¢«ç¼è¾è¿çæä»¶ð
- `-mtime` means modified time 
- `-1` means last day 
> ```
> $ find . -mtime  
> ```

> 4. Find the files and delete them //å¯»æ¾æä»¶å¹¶å é¤
- `*.tmp` all the temporary files
- `-exec rm`execute them with remove command 
> ```
> $ find . -name "*.tmp" -exec rm {} \;
> ```

> 5. Find files by sizes //æ ¹æ®æä»¶ðå¤§å°å¯»æ¾
> ```
> $ find . -size +500k -size -10M -name '*.tar.gz' # Find all zip files with size in range 500k to 10M
> ```

### ðWhat is shebang?
> Different names: //ä¸åå
#### It is also called `hashbang, pound-bang, or hash-pling.`
#### Always starts with `#!` at the beginning of a file.
> Objective: //ç®æ 
#### Increase the portability of the script. It will make use of the `PATH` environtment and resolve to wherever the command lives in the system. //æåèæ¬çä¾¿æºæ§è½ï¼ååå©ç¨`PATH`ç¯å¢ æ¾å°ç³»ç»ä¸­å­å¨ç âå½ä»¤â

> Example: //ä¾å­
#### `#!/usr/bin/env python3`, Execute with a Python interpreter, using the `env` program search path to find it. //æ§è¡python ç¼è¯å¨ï¼ä½¿ç¨ `env` ç¨åºæç´¢ðè·¯å¾

### ðArguments //
#### As you see the above case, there are other definitions of arguments in bash.
- `$0` - Name of the script //èæ¬å
- `$1` to `$9` - Arguments to the script. `$1` is the first argument and so on  //Arguments å°script 
- `$@` - All the arguments  //ææarguments
- `$#` - Number of arguments  //argumentsçæ°é
- `$?` - Return code of the previous command. Similar to `return 0` in C++  //
- `$$` - Process identification number (PID) for the current script //å½åèæ¬ç è¯å«ç 
- `!!` - Entire last command, including arguments. A common pattern is to execute a command only for it to fail due to missing permission; you can quickly re-execute the command with sudo by doing `sudo !!`  //è¾å¥ä¸ä¸ä¸ªæä»¤ï¼åæ¬arguments
- `$_` - Last argument from the last command. If you are in an interactive shell, you can also quickly get this value by typing `Esc` followed by `.` //
  
### suppose you are at `~`
> ```
> $ mkdir /mnt/new  #the prompt will say you don't have permission
> $ sudo !!  #Here means `sudo mkdir /mnt/new`  
> ```

### Frequently Used Commands //å¸¸ç¨å½ä»¤
|Command  å½ä»¤|Objective  ç®æ |Example  ä¾å­|
|---------|-----------|---------|
|`cat`    |catch whatever inside  ææ|`cat hello.txt`|
|`cd`     |change directory æ¹åç®å½|
|`cp`     |copy a file  å¤å¶ä¸ä¸ªæä»¶|
|`echo`   |like "echo", it simply prints out its arguments  æå°åºarguments|
|`find`   |find <folder> -name <name> -type <type>|`find . -name main -type f`|
|`fd`     |shortcut for find(not installed yet) findçå¿«æ·é®|`fd "*py"`|
|`history`|list the history of your typed bash commands bash å½ä»¤åå²|
|`ls`     |list all the files in current directory  ç½åå½åç®å½ä¸çæææä»¶ð|
|`man`    |manual of something  ç®å½|`man rm` |
|`mkdir`  |make a directory/folder  åå»ºä¸ä¸ªç®å½/æä»¶ð|
|`mv`     |rename/move a file éå½å/ç§»å¨ä¸ä¸ªæä»¶|`mv xx.md yy.md`|
|`pwd`    |present working directory å±ç¤ºå·¥ä½è·¯å¾|
|`rm`     |remove a file  ç§»é¤æä»¶|
|`rm -r`  |remove all the files recursively éå½å°ç§»é¤æææä»¶|
|`rm -rf *` |remove all the files at the current folder å¨ç°æçæä»¶å¤¹ðä¸­ç§»é¤ææçæä»¶ð|
|`rmdir`  |remove EMPTY folder|`rm ./.vscode`|
|`rg`     |R.I.P |`rg "import" -t py ~/dev`|
|`tail`   |print the last _n_ lines æå°æånè¡  |tail -n3|
|`touch`  |create a file åå»ºä¸ä¸ªæä»¶ð ï½touch main.cpp |
|`shellcheck`|Debug bash file |`shellcheck mcd.sh`  |
|`<command> --help`|see the function of this command å½ä»¤|`git --help`
|`man <command>`|open the menu of this command æå¼å½ä»¤çèå|`man ls`|
|Ctrl+L|clean out the shell|
