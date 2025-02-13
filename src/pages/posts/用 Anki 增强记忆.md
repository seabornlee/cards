---
date: "2014-05-11"
---

<img src="/_image/2014-05-11/22-47-48.jpg" alt="">

Anki is a program which makes remembering things easy. Because it's a lot more efficient than traditional study methods, you can either greatly decrease your time spent studying, or greatly increase the amount you learn.

Anki 是一个帮助记忆的应用，它使用卡片的形式，正面是题目，反面是答案。根据记忆的程度调整复习的周期。
我这次想用 Anki 来帮助我记忆[git plugin of oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh/blob/master/plugins/git/git.plugin.zsh)提供的 git 别名，首先要做的事情就是将这些别名导入到 Anki 中。

### 文件格式

Anki 需要的文件格式如下，每行表示一个卡片，分号前面是卡片正面的文字，分号后面是卡片背面的文字：

```
git status;gst
git add;ga
```

[git plugin of oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh/blob/master/plugins/git/git.plugin.zsh)的 shell 文件如下：

```
# Aliases
alias g='git'
compdef g=git
alias gst='git status'
compdef _git gst=git-status
alias gd='git diff'
compdef _git gd=git-diff
alias gdc='git diff --cached'
......
```

不难看出，只要稍做加工便可以达到我们的目标。首先过滤出 alias 开头的行，去掉 alias 关键字，再将等号左右的内容对调并且将等号替换为分号就可以了。

### awk 出场

首先将文件下载下来：

```
curl https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/plugins/git/git.plugin.zsh -o git.plugin.zsh
```

然后执行如下命令：

```
awk '/^alias/' git.plugin.zsh | awk '{split($0, arr1, "="); split(arr1[1], arr2, " "); printf "%s;%s\n", arr1[2], arr2[2]}' > deck.txt
```

这是我第一次尝试 awk，被其深深折服了。我相信这个脚本一定不是最优的，如果您有更好的方法请一定留言告诉我：

```
awk '/^alias/' git.plugin.zsh
```

筛选出`git.plugin.zsh`中以 alias 开头的行；

```
split($0, arr1, "=")
```

将第一条命令的结果按等号拆分，并且存放到以 arr1 命名的数组中；

```
split(arr1[1], arr2, " ")
```

将 arr1 中等号左边的部分以空格为分隔进行二次拆分，并且存放到以 arr2 命名的数组中；

```
printf "%s;%s\n", arr1[2], arr2[2]
```

打印等号右边的部分，打印分号，打印空格右边的部分；
最后将命令结果重定向到文件`deck.txt`中。

### 导入 Anki

按格式生成好文件之后，导入就非常简单了。
打开 Anki，按 Command+i 或点击“File->Import”，选择`deck.txt`即可。
![导入Anki](/_image/2014-05-11/22-00-34.jpg)

### 参考资料

[AWK 简明教程](http://coolshell.cn/articles/9070.html)

### Update

---

![Juven评论](/_image/2014-05-12/07-58-38.jpg)
不出我所料，[Juven](http://www.juvenxu.com/)果然有更好的方法。

```
grep '^alias' git.plugin.zsh | sed 's/^alias //g' | awk -F '=' '{print $2";"$1}'
```

主要改进两点：

-   使用 sed 将“alias ”替换掉;
-   用`-F '='`代替 split；

经过这样的改进，意图更加清晰，也更符合 linux 思维。

又一次印证了那句老话：

> 当你手里只有一把锤子，你看什么都像钉子。

因为之前并不知道 sed 可以轻易帮我做文本替换，所以蹩脚地用 awk 的 split 来达到这个目的，牺牲了可读性。
