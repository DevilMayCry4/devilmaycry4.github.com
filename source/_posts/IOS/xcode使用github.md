title: xcode使用github
date: 2013-10-26 14:02:18
tags: IOS
---
<p>
Xcode 中使用Github
第一步：为你的mac添加认证，使得它能够连接到github。

参考http://help.github.com/mac-set-up-git/ 一步一步操作即可
====================================================================
第二步：新建项目
先在github里面添加一个Repository，通过http://github.com/首页的new Repository连接进入，填写相关的项目信息，创建即可。创建后会跳转到初始项目界面，先暂停，在第三步再使用这个界面。
在mac上使用xcode新建一个项目，为项目使用git，创建过程中使用 create local git repository for this project，指定要存储的目录即可。
====================================================================
第三步：初始化提交项目
回到第二步github创建项目后的界面。初始化有几种方式，这里我们选择从已有仓库提交代码。执行下面的代码即可。
Existing Git Repo?
cd existing_git_repo
git remote add origin git@github.com:your_account/your_project.git
git push -u origin master
上面的代码将本地的仓库连接到远程仓库，并且将代码提交到master分支上。
====================================================================
第四步：提交代码
新建的文件，XCode会提示一个A标签，表示是added的。
编辑的文件，XCode会提示一个M标签，表示是Modified的。
选中要更新的文件，右键—>Source Control—>Commit Selected Files 通过此操作将变更提交到本地的仓库中。
选中要更新的文件，File菜单—>Source Control—>Push 将本地变化存储到远程服务器中。

</p>
<p>
from:http://qiufangzhou.blog.163.com/blog/static/506421802011924102515390/
</p>