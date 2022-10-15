---
title: 在XCode中编译下载的项目
date: 2018-02-10 23:59:14
tags:
- Swift
- Complie
- Objective-C
categories:
- Code
---
# Foreword
  Sometimes, we need to compile others' program, but to the people who didn't know Swift or Objective-C at all, compiling other people's program is so difficult to do. So this passage will tell you how to compile existed project in XCode.

<!--more-->

# Contents
  > - [Prepare](#Prepare)
  >   - [Carthage](#Carthage)
  >   - [CocoaPods](#CocoaPods)
  > - [Compile](#Compile)
  >   - [Clone Project](#Clone-Project)

# Prepare
  There are two ways to install project's dependency, so judge it from project files.

  You may saw some projects tree like this: ![Cartfile Project][1]
  Look at the last two files, they start by `Cartfile`. You need to process these project in the step [Carthage](#Carthage).

  Or you saw others are like this: ![CocoaPods Project][2]
  You can see `Podfile` in this project, to complie this kind of projects, see step [CocoaPods](#CocoaPods).

## Carthage
  Carthage is a light dependency installer for Swift or Objective-C. Different from CocoaPods, it installes the packages whichever you need but not all packages.

  For more information, see [Carthage][3].

  First, you should install brew, a software which can patch your macOS with some open source packages.
  ```bash
  This Command Line Will Install Homebrew.
  ```
  Then, execute it to update your brew.
  ```bash
  brew update
  ```
  Last, install Carthage by
  ```bash
  This Command Line Will Install Carthage.
  ```

## CocoaPods
  CocoaPods is a long-standing third party package manager, but it is too huge.

# Compile
  First, you should download the XCode from App Store. And you should start it to finish the first runing config and download the `Command Line Tools for XCode`.

## Clone Project
  Clone your project from github in XCode, you should press the button `Clone an existing project`.![XCode Main Window][4].

  In this window, you should fill the textfiled with your project link, like `https://github.com/hulizhen/cuImage` or `ssh://git@github.com:hulizhen/cuImage.git`.![Clone Window][5]

  > Notice: You can fill your github account link in the textfiled and check the project you stared. Like this ![Stared Window][6]

  Press `Clone`, you will get the project.

  > ****NOTICE: THIS BLOG IS NOT FINISHED, AND SOMETHING MUST BE WRONG BECAUSE I DID NOT TEST IT AT ALL****



[
