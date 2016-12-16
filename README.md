# GIT

某种GIT实践的莞式方案。

# 综述

Git有许多实践方案，本着自由开源的思想，本方案多数规则不强制，在项目实践过程中可自行修改演变。**Take it easy :)**

# Labels

首先定义项目的`Label`，这些`Label`可以作为Git管理系统的检索的重要依据。道理大家都懂，制定的时候难免要考虑，下面直接给出。各位也可在实践中自行添加删除。

1. Commits行为

    这些`Label`将在申请PR时和提出Issue时使用。

    * `feature`
    * `refactor`
    * `bugfix`

2. Iusse类型

    这些`Label`将在Issue提出时使用

    * `question`
    * `discuss`

添加`Label`的原则：

1. 一个行为(PR, Issue)可以有多个`Lebel`，但上述类型中最好同种类型只用一个`Lebel`。
2. 只要是项目成员，都鼓励合理地添加删除`Label`，别想着捣乱，Git管理系统都会记录下来的。
3. `Label`不强制，Git不喜欢一本正经 :)

# Branches

## 基础分支

所有项目推荐始终包涵`master`以及`develop`分支，这部分将在[PR](#PR)部分中详细说明。

## 命名

使用如下格式进行分支命名：
```shell
[commits type]/[short-description]
```

举个例子：
```shell
$ git checkout -b feature/add-some-component
```

好处（可以不看）：某些Git管理系统在URL中将分支名包含其中，如果以类似`feature/...`开头，那么会形成类似`gitxxx.com/.../branches/feature/...`的URL，一来一目了然，二来方便检索。

## 何时新建分支

本方案中建议始终从`develop`创建分支，好处是能将产生冲突的可能性减少到最小，并且分支网络(Network)将变得精简，不交叉，十分漂亮，

当然从某个非基础分支创建的分支也无所谓，只要解决了冲突，对代码来说都是一样的，Git正是为此而生。

# Commits

## Commit Message

随便写点啥来描述这个commit，建议以commit类型开头，比如下面这样（注释部分不算Commit Message，这是git-core自动生成的提示内容）：

```shell
feature. add some component.
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch develop
# Changes to be committed:
#       modified:   some-component.js
#
~
```

## 何时创建Commit

很多Git学院派认为Commit的创建需要遵循一定规则，因为Commit Message将现实在Git管理系统对应的每个文件后面。而我的建议是：`凭直觉`，一般是一个功能完成即可形成一个Commit，多个Commit可以直接推送至线上分支，也可以一个一个推送。

# Issue

Issue是Git管理系统的社区职能的主要承担者，它十分单纯，类似一个有些延伸功能的BBS。开发人员可以随意提出Issue，但是需遵守以下原则：

1. 提交Issue之前看看有没有其他人提交了相同的Issue，如果有，那么直接在原Issue下`+1`即可。
2. 不要在人家的Issue的评论中提出新的Issue。
3. 不要在同一个Issue中报告多个问题。

另外不要忘记加`Label`哦。

## 使用模版

许多Git管理系统支持Issue和PR模版，实现方式各有不同，但是对于问题收集和定位十分有效。可以尝试在本项目中提Issue看看效果。

# PR

PR(Pull Request)是Git流程中最重要的一环，它决定了Git是否真正作为版本控制工具来进行使用。

> 小插曲
>
> 有些Git管理系统比如Github，Bitbucket叫PR(Pull Request)，有些比如Gitlab和Gitorious叫MR(Merge Request)，大家普遍叫PR因为Github出来最早，而后来Gitlab改名叫MR之后，大家都很困惑，问Gitlab之后它特意写了[一段话](https://about.gitlab.com/2014/09/29/gitlab-flow/)来说明：
>> Merge or pull requests are created in a git management application and ask an assigned person to merge two branches. Tools such as GitHub and Bitbucket choose the name pull request since the first manual action would be to pull the feature branch. Tools such as GitLab and Gitorious choose the name merge request since that is the final action that is requested of the assignee. In this article we'll refer to them as merge requests.
>
> 大意是说虽然名字不同，但是大家都是一个意思，只不过一个强调行为的开头（Pull），一个强调行为的结尾（Merge），据说小道消息是为了防止法律纠纷。

PR行为不仅仅是提出PR开始，事实上从提出Issue开始，已经开始了PR，所以从这一点来说MR更加准确一点。

## 委派

提出Issue的时候，若是讨论结果需要进行代码修改，建议使用`Assignee`功能委派给相应开发人员，这不仅仅是一个标记功能，告诉团队成员此需求已被相应人员处理，并且Git管理系统将会给相应人员发送邮件提醒。

## 关联

Issue和PR都可以关联彼此和人员，原则上相关的Issue、PR和人员需要相互关联，使用`#`关联Issue和PR，使用`@`关联人员。

## 分支管理

所有开发人员可以自由地创建分支，建议从`develop`分支新建分支，并且合并到`develop`。`develop`分支将作为默认分支，而不是`master`。

`develop`和`master`分支将被设置为保护(protectd)：

* 需要特定人员来最终合并
* 需要在CI通过之后才允许合并
* 禁止非特定人员使用`push`命令
* 禁止所有人员使用带`-f`的`push`命令

## Code Review

当有一个PR提出时，一般需要有Code Review，可以约定一个数量，例如必须有2个人Review之后，才允许合并。PR提出人员可以在PR描述中使用`@`来关联Review人员，他们同样会收到邮件。

## CI

所有合并到基础分支的PR都需要通过CI，而合并到其他分支的PR不要求。  
只有合并到`master`分支的才会部署到生产环境，合并到`develop`分支将进入测试环境。

# 版本演进

版本演进是看似简单，其实挺玄学的事儿，我们既然是莞式方案，那么我们直接给出一个全能简单的方案。

## 版本号

版本号一般是有特殊规定的，可以如下：

* 版本号格式为`x.y.z`
* x代表**巨大功能革新**，**底层技术更迭**及**UI版本剧变**
* y代表**新功能上线**，**代码逻辑变更**，**功能重构**，**巨大Bug修复**及**一般UI变动**
* z代表**一般Bug修复**及**UI微调**

对于项目发布周期有如下描述：

* `0.0.x`版本均为`pre-alpha`版本，也是技术体验版，这个版本**不承诺任何功能**，可能出现**巨大的BUG**，**中英文混用**以及**外观丑陋**等问题
* `0.1.x`版本均为`alpha`版本，也是内部测试版，这个版本**大部分功能稳定**，基本上完成交互，并经过一定的测试
* `0.x.x`版本除去上面两个类型的版本号之外的所有版本号均为`beta`版本，也是公共测试版，这个版本面向大众，任何版本都有可能成为正式发布候选版本（RC版）
* `1.0.0`版本是正式发布的第一个版本（GA版）
* 其后所有版本都为正式版本，修改遵循版本号规则。

## Release

一般来说，为了能迅速找到任何一个版本的最终代码产物，随着版本号演进，每个版本都需要释出相应的Release版本，但是事实上很少有项目真的如此做，多半是许多修改集合到一个Release中发布，使得版本号进度不至于过快。

## Tag

标签是Git十分人性化的一面，可以使用`tag`命令，它将标记一个commit为一个给定的别名，而不是一传hash代码。这可以直接标记出项目开发中的几个关键版本，而不需要一直保留这些分支。

## 里程碑

很多国内网站将里程碑等同于`tag`，这其实是错误的，里程碑（milestone）事实上是很多Git管理系统提供的功能，它并不像`tag`那样标记某个commit，而是为了标记一连串的commit，这些commit组成了项目演进过程中某个路径，里程碑可以将这些commit联立起来。

里程碑可以设置描述和起始终结时间，是向领导汇报工作的利器 :)