### 更新环境变量


您在命令行只能更新当前会话的PATH变量，如[Clone Flutter repo](./#clone-the-repo)所示。
但是，您可能需要的是永久更新此变量，以便您可以运行`flutter`命令在任何终端会话中。

对于所有终端会话永久修改此变量的步骤是和特定计算机系统相关的。通常，您会在打开新窗口时将设置环境变量的命令添加到执行的文件中。例如

1. 确定您Flutter SDK的目录，您将在步骤3中用到。
2. 打开(或创建) `$HOME/.bash_profile`. 文件路径和文件名可能在您的机器上不同.
3. 添加以下行并更改`[PATH_TO_FLUTTER_GIT_DIRECTORY]`为克隆Flutter的git repo的路径:


```commandline
export PUB_HOSTED_URL=https://pub.flutter-io.cn //国内用户需要设置
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn //国内用户需要设置
export PATH= PATH_TO_FLUTTER_GIT_DIRECTORY/flutter/bin:$PATH
```

注意：`PATH_TO_FLUTTER_GIT_DIRECTORY` 为你flutter的路径，比如“~/document/code”

```commandline
 export PATH= ~/document/code/flutter/bin:$PATH
```

4. 运行 `source $HOME/.bash_profile` 刷新当前终端窗口.

{% include note.html content="如果你使用的是zsh，终端启动时
`~/.bash_profile` 将不会被加载，解决办法就是修改 `~/.zshrc` ，在其中添加：source ~/.bash_profile"
 %}

5.通过运行`flutter/bin`命令验证目录是否在已经在PATH中:

``` commandline
echo $PATH
```

更多详细信息，请参阅[this StackExchange question](https://unix.stackexchange.com/questions/26047/how-to-correctly-add-a-path-to-path).
