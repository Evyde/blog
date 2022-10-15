---
title: 安装与备份Hexo
date: 2018-02-05 20:53:29
tags:
- Hexo
- Backup
- Software
categories:
- Z-Turn
---
# Foreword
  About three years ago around 2015, I found I need to record something important by writting blog. But I didn't prefer to buy VPS or Hosting, and it is too hard to do it in free hostings. So I found github(gitcafe) and Hexo.
  - What is Hexo?
  It is a fantastic program which can help people creat their static blog page easily.

  - How Hexo works?
  It relys on Node.js, and can generate static html pages in your local device.

  - Why is Hexo?
  Fast, beautiful themes, easy to use, etc.

  This is the home page from my blog, in which you can see how beautiful it is.
  <!--more-->
  ![IndexPic][2]
  Then, if you want to know more about Hexo, you can check details in its wiki page, see [Hexo Wiki Page][1].

# Contents
  > - [Prepare](#Prepare)
  > - [Installation](#Installation)
  >   - [Install Node.js](#Install-Node-js)
  >   - [Install Git](#Install-Git)
  >   - [Install Git Deployer](#Install-Git-Deployer)
  > - [Simple Usage](#Simple-Usage)
  > - [Backup](#Backup)
  > - [Restore](#Restore)

# Prepare
  > - A Linux or Mac computer.(Though Windows is ok, but I don't suggest you use it, if you still go on working on it, you will have a bad enough experience.)
  > - A github account.
  > - Be familiar with Markdown.
  > - Maybe a good VPN.

  Now, let's see how to install and use Hexo.

# Installation
## Install Node.js
  Download Node.js from its official website [Node.js][3], and follow its guide to install it. Now you get npm installed. To test whether the npm successfully installed, you can input
  ```bash
  npm -v && echo $?
  ```
  Then you will see a number 0, if not, check your Node.js installation.

## Install Git
  In macOS, you just need to input command such as `gcc` or `git` in terminal, then macOS will ask you if you need to download developer command line tools, just choose `Yes`.
  In Linux, you can use package control system like `apt` or `yum`.
  To check if git works, input
  ```bash
  git -v && echo $?
  ```
  Like last step of installing Node.js, you should see a number 0 now, too. If not, check [github][4] for more information.

## Install Hexo
  Just input
  ```bash
  npm install -g hexo-cli
  ```
  to install hexo on your system. Input
  ```bash
  hexo -v && echo $?
  ```
  to check if it installed successfully. For more infomation, you can go to [Hexo Wiki Page][1].

  From now on, you finished hexo installation, let's see how to use it.

  Before use it, you should creat a workspace for you blog at first.
  ```bash
  mkdir blog
  cd blog
  ```
  Unless special mention, we will use this directory by default.

## Install Git Deployer
  You need to install a git deployer to push your blog into github, Input
  ```bash
  npm install hexo-deployer-git --save
  ```
  > Notice: You should reinstall it whenever you have a new blog.

  Then, modify your `_config.yml` to make it avaliable. Add these contents at the end of the file.
  ```yaml
  deploy:
    type: git
    repo: <repository url>
    branch: [branch]
    message: [message]
  ```
  > Notice: This file is in your blog directory.

  | Argument    | Description                         |
  | :-:         | :-:                                 |
  | type        | The type of deployer, here is git   |
  | repo        | Your git repository url, ssh or http|
  | branch      | Repository branch, like `master`    |
  | message     | Commit message, usually don't mind  |


# Simple Usage
  Initialize your blog by inputing
  ```bash
  hexo init
  ```
  After a while, you can get a default blog. But you need to modify `_config.yml` to customlize your blog.

  | Argument    | Description              |
  |    :-:      |            :-:           |
  | title       | Website title            |
  | subtitle    | Website subtitle         |
  | description | Use for SEO              |
  | author      | Blog author, your name   |
  | language    | Your website language    |
  | timezone    | Don't mind it            |
  | url         | Your blog url            |
  | theme       | Which theme you use      |

  > Notice: Every argument follow the syntax like this:
  ```yaml
  theme: landspace
  ```
  > There is A `SPACE` between the symbol `:` and `landspace`.


  Now, you can creat you first passage by
  ```bash
  hexo new [layout] <title>
  ```
  And you will see a new file in `./source/_post/`, modify it to draw your think.
  > Tips: If you want to add a jumpable content in your passage, you can do like this( don't need use html label).
  ```markdown
  [Guide1.1](#Title)
  ```
  > In this example, `Guide1.1` means the thing you want to show to readers. `Title` means the target where you want to jump, for example, in this passage, `Prepare` is a title, so I should write my content like this
  ```markdown
  [Prepare](#Prepare)
  ```
  > > Notice: If you meet a title which has more than one word, like `Install Node.js` in this passage, you should replace all the special symbols by `-`, for example
  ```markdown
  [Install Node.js](#Install-Node-js)
  ```
  or you can't reach your target.

  When creative things compelete rapidly, generate your passage by
  ```bash
  hexo g
  ```
  and submit by
  ```bash
  hexo d
  ```
  If you want to see its effact without submitting, input
  ```bash
  hexo s
  ```
  after generating. And you will see a message like
  ```bash
  (node:27254) [DEP0061] DeprecationWarning: fs.SyncWriteStream is deprecated.
  INFO  Start processing
  INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
  ```
  Then, go `http://localhost:4000/` to see your blog in local device.
  Also, there are two useful commands for you.
  ```bash
  hexo clean
  ```
  It means clean the generated files. You can use it whenever you have something wrong to deal with.
  ```bash
  hexo list
  ```
  List your website information.

# Backup
  First, you should creat an **another empty** repository in github. Then initialize git by inputing
  ```bash
  git init
  ```
  Then, add your blog files to the git repository by
  ```bash
  git add .
  ```
  > Notice: There is an `.` after `add`.

  Commit by inputing
  ```bash
  git commit -m "[Message]"
  ```
  You can replace `[Message]` by anything you like, it is the message submited to github to show the feature of this commit.
  Then, remote your repository by inputing
  ```bash
  git remote ssh://git@github.com:XXX/blog.git
  ```
  > Notice: `ssh` means the type which the deployer will use, if you add the public RSA key to the github, you can use this method, or change it to `https://`. `XXX` means your github username, and `blog` means your repository. If you use https method, you should write it in this way
  ```bash
  git remote https://github.com/XXX/blog.git
  ```

  Now finish your first backup by
  ```bash
  git push origin master
  ```
  `master` means the brach of your github repository, you should use it by default. Before changing it, you should creat a new brach in the github website.



# Restore
  If you have a new computer or you move your workspce, you need to restore your blog.
  First, you should install `git` and `Node.js`. And creat a new workspce by inputing
  ```bash
  mkdir workspace
  cd workspace
  ```
  > Notice: If your directory name is too long to type, just use `TAB` to help you.

  And clone your backuped blog by
  ```bash
  git clone ssh://git@github.com:XXX/blog.git
  ```
  Then, exec
  ```bash
  npm install hexo --save
  ```
  npm will install hexo in this directory automatically.
  By the way, install git deployer in this directory, see [Install Git Deployer](#Install-Git-Deployer).

  Now, you have your blog restored.

  > ****NOTICE: THIS BLOG IS NOT FINISHED, AND SOMETHING MUST BE WRONG BECAUSE I DID NOT TEST IT AT ALL****






  [1]: http://hexo.io/

  [3]: http://nodejs.org/
  [4]: https://github.com
  [5]: http://
