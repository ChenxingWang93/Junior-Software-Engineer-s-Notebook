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

## 📍 modeling history: relating snapshots
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


