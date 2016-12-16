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

所有开发人员可以自由地创建分支，建议从`develop`分支新建分支，并且合并到`develop`。

`develop`和`master`分支将被设置为保护(protectd)：

* 需要特定人员来最终合并
* 需要在CI通过之后才允许合并
* 禁止非特定人员使用`push`命令
* 禁止所有人员使用带`-f`的`push`命令

## Code Review

当有一个PR提出时，一般需要有Code Review，可以约定一个数量，例如必须有2个人Review之后，才允许合并。
