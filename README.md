# mylangchain
mylangchain study for you
### DayOne 
## 安装
- install vscode ，插件GitHub Pull Requests and Issues，插件Gitlens
- install git https://git-scm.com/install/windows  如果选择了master 可以用下面命令改
PS C:\WINDOWS\System32> git config --global init.defaultBranch main
PS C:\WINDOWS\System32> git config --global --get init.defaultBranch
- 重启vscode
- 注册Github， 建立仓库mylangchain
- 在vscode 的命令行克隆 git clone https://github.com/fireflygoup/mylangchain.git
- 克隆成功后， 修改readme.md  
- 上传去这个目录上传 git push https://github.com/fireflygoup/mylangchain.git main  出现Everything up-to-date
- 第二种push 做origin如vscode记住地址。  git remote add origin https://github.com/fireflygoup/mylangchain.git  在打 git push -u origin main
- 第一次commit 需要：
git config user.name "fireflygoup"
git config user.email "chen_xuefeng@hotmail.com"


- git add . → 所有改动放进暂存区（新项目首选） 
- git commit -m "init:项目初始化"
- git push https://github.com/fireflygoup/mylangchain.git main

- 如果有冲突 那么 git pull https://github.com/fireflygoup/mylangchain.git main --allow-unrelated-histories
- 如果有冲突  用这个不用条vim的。git pull https://github.com/fireflygoup/mylangchain.git main --allow-unrelated-histories -m "合并远程main分支代码"
testing