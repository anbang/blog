- git stash
    - 保存当前的工作进度。会分别对暂存区和工作区的状态进行保存

- git stash save "message..."
    - 这条命令实际上是第一条 git stash 命令的完整版

- git stash list
    - 显示进度列表。此命令显然暗示了git stash 可以多次保存工作进度，并用在恢复时候进行选择

- git stash pop [--index] [<stash>]
    - 如果不使用任何参数，会恢复最新保存的工作进度，并将恢复的工作进度从存储的工作进度列表中清除。
    - 如果提供参数（来自 git stash list 显示的列表），则从该 <stash> 中恢复。恢复完毕也将从进度列表中删除 <stash>。
    - 选项--index 除了恢复工作区的文件外，还尝试恢复暂存区。

- git stash apply [--index] [<stash>]
    - 除了不删除恢复的进度之外，其余和 git stash pop 命令一样

- git stash clear
    - 删除所有存储的进度