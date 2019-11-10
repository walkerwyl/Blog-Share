# SWUFE 奇点工作室学习博客交换站

为切实提高工作室成员的学习积极性、涉猎宽度及思考的深度，从本学期开始，奇点工作室倡议每位成员 **每周至少完成一篇** 博客的写作及分享。涉及计算机基础理论、数学学习、数据分析、网站开发、网页设计、金融科技等等方面的学习经历和收获，也有相互之间的交流与讨论。当然，我们非常欢迎工作室之外的朋友们对我们的思考和学习笔记提出指正意见，也希望更多志同道合的朋友可以参与到我们这一次的尝试与努力中来，我们愿意相信，**一个人走得快，一群人走得远**，希望我们可以在柳湖旁一起建造一片GEEK之林！

博客地址：[SWUFE 奇点工作室学习博客交换站](https://singularity-lab.github.io/Blog-Share)

## 内容及形式

每一篇博客可以是分享自己的项目，也可以是某个知识点的总结回顾，也可以是学习笔记，也可以是一周之内遇到的细碎问题合集或是重构笔记等等。

或许这是一个可供参考的例子：[Javascript Fetch API to send data](https://medium.com/@whole9681/8c2b1dedaba)

**感谢我院优秀学姐学长对奇点工作室的大力支持！**

## 格式规范

1. 每篇博文中都有一个 `header` 部分，其中包含：名字或昵称、日期、标签（如，Django, 网络协议，机器学习算法，数据分析实践，Python等等）

   ```markdown
   ---
   layout:     post
   title:      "图表征学习算法_node2vec"
   date:       2019-05-05
   author:     "林柯秀"
   header-img: "img/post-bg-2015.jpg"
   tags:
       - 图表征学习
       - 特征工程
   ---
   ```

2. 文章命名： `{日期yyyy-mm-dd}-{文章标题}` ，如： `2019-03-04-在Console中添加字符画`

3. 图片等静态资源请放置于： `img/[年月]/[周数]/[名字/昵称]` ，如： `img/1903/04/Hivol/`

   ```bash
   img
   └── 1903
       ├── 03
       │   ├── Hivol
       │   ├── YanYijun
       │   ├── iyiniyin
       │   ├── lxy
       │   └── th
       └── 04
           ├── Hivol
           ├── Mialia
           ├── iyin
           ├── lxy
           ├── shuangmulin
           ├── thang
           └── walkerwy
   ```

4. Markdown 文件中引用图片时，请使用绝对路径。如： `/Blog-Share/img/1903/04/Hivol/hello.png`

5. Markdown 文件中也请注意可读性与美观性，建议在标题前后、段落前后和图片前后均空一行，建议 [使用 Prettier](https://prettier.io/)

   ```bash
   yarn add prettier --dev --exact
   
   yarn prettier --write _posts/2019-05-05-图表征学习算法_node2vec.md
   ```

6. 使用 UTF-8 编码

7. Pull Request 三要素：Theme, Summary and Details。Pull Request Summary 的格式为： `{年月周}-{文章标题}-{姓名}` ，如：190304-Django项目上线部署全流程-LKX

8. **文末请注明参考文章及链接**

## 怎样上传新文章？

Step 1. Fork 本项目得到 `nickname/Blog-Share` （此处的“nickname”即是各位自己的用户名）

Step 2. Clone 你的仓库到本地电脑得到 `local/Blog-Share`

Step 3. 在本地仓库中编写你的文章，写完后 commit（ ⚠️ 注意：blog 在 `_posts` 文件夹下，图片在 `img` 文件夹下）

Step 4. push `local/Blog-Share` 到 `nickname/Blog-Share`

Step 5. 获取本项目最新内容，与本项目进行同步：

- 确认 `local/Blog-Share` 中已设置好 upstream 为本项目地址（ `git remote -v`  查看），正常情况下与下图一致（"Hivol"应该是自己 GitHub 的用户名），如果没有请使用 `git remote add upstream https://github.com/Singularity-lab/Blog-Share.git` 添加。

  ![readme-img1](img/readme-img1.png)

- 使用 `git status`  检查本地是否有未提交的修改，如果有，请先 commit 你的提交，并 push 到 上文提到的 `nickname/Blog-Share` 

- 执行命令 `git fetch upstream`  获取本项目的最新更新内容

- 执行命令 `git checkout master` 切换到 master 分支（或你的默认分支下，如 gh-pages）

- 执行命令 `git merge upstream/master` ，把本项目的最新内容合并到你的本地仓库的 master 分支（或你的默认分支下，如 gh-pages）中

- 执行命令 `git push` 把合并后的内容 push 到你的远程仓库  `nickname/Blog-Share` 

Step 6. 提交一个 pull request，等待 review 之后合并

## 本地运行调试

1. 安装 ruby 及其包管理器 gem
2. `gem install bundler`
3. `bundle install`
4. `jekyll serve` 或 `bundle exec jekyll serve`
5. 按照命令行提示打开浏览器页面即可

## 转载须知

1. 转载请联系（Email: Singularitylab@163.com; QQ: 171445684）
2. 转载请注明奇点工作室和个人署名
