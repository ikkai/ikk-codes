# 分支管理规范

我们的项目仓库托管在Github上。开发者可以在自己的机器上安装[Github Desktop](https://desktop.github.com/)配合使用。

### 分支类型：

- master：主开发分支。团队成员一般不会直接更改该分支，而是分别从该分支检出自己的个人分支，开发完成后将个人分支上的改动merge回master分支，merge需要通过给项目负责人发送Pull Request的方式进行。同时release-stg以及release-prod分支由此分支检出。主分支不集成服务器任何环境，仅做开发用。若master分支发现bug，则负责修复bug的人需要将master分支检出到自己的分支上进行修复，修复完成之后，合并到master分支。从master分支检出之后，需要时刻注意master分支的变化，如果有别人推送了代码，需要及时的把master拉回本地看看是否有冲突，将冲突提前解决。推荐使用Github Desktop软件，将git fetch操作持续进行。一般保证每天下班前将代码Pull Request给至少两名团队成员进行Code Review（Approve + 2）。评审者将在github的PR页面上和团队成员进行PR的相关讨论。如果PR未通过，则需要成员继续完善并commit，然后发新的PR给评审者。如果PR通过则会merge到master，消息服务（我们暂且用的是worktile）会通知项目成员，大家即可pull一下master看看是否有冲突。开发人员的本地机器需要对master分支进行状态监控，当有改动的时候，需要进行本地测试。
- release-stg：预备环境分支。该分支CI集成服务器预备环境。主要用于将master上开发完成的功能，推送至线上预备环境，进行预发布。同时，该分支也用于用户参与封闭测试。产品经理与测试人员先从master分支检出一个稳定的commit，仅进行功能性测试，测试完毕之后，merge到release-stg, 打tag, 然后推送远端。此时CircleCI会运行线上测试, 测试通过即可集成到预备环境中。一般该分支，每周至少保证推送一次。一般是一个功能完成之后，即可推送至该分支。运营，封测用户等，可以在该环境中测试。开发者不能直接操作该分支。
- hotfix：热补丁分支。release-stg或者是release-prod分支出现了bug，从release分支中，检出到hotfix分支，进行修复，修复完成之后修bug的人发PR给主管请求合并。主管通过之后即可合并到release分支。之后该分支即可删除。检出hotfix的时候，注意命名。一般为 ```hotfix-bug的名称```, 例如：```hotfix-用户权限错误``` 
- release-prod: 生产环境分支。该分支集成服务器生产环境。主要用于将预备环境中，经过了单元测试，e2e测试的功能，推送至线上生产环境中。一般保证一周至少推送一次该分支，该测试面向生产用户。请务必谨慎对待。开发者不直接操作该分支。由产品经理与测试将stg检出至该分支，然后推远端，发布成功后打tag，然后把tag也推远端。

### 常见问题：

1. **忙活一大早，发现把代码写到了release分支怎么办。。。**

   请百度一下stash功能怎么用。

2. **在自己的分支上开发功能，总是习惯性的commit，然后推一下，再写一行，再推一下。**

   请克服强迫症，保证每一个commit都是有意义的。不要产生过多的无意义的commit

3. **我写的功能，需要改动一个全局的组件，这个组件有可能其他人也要用到。我一改其他人估计都会冲突了。**

   如果全局的一些底层组件，接口设计的有问题，不满足后续的业务发展。需要全体开发人员开会，确定新的组件接口。然后由负责该组件的成员，实现该接口。然后推送代码至master，所有收到影响的同事从master检出，然后完成之后在合并到master。

4. **hotfix修bug，没修好，结果把整个项目都弄崩溃了**

   github上，merge PR之后，可以revert，revert一下就回滚了。

*其他问题，欢迎大家补充*



> 参考文档：
>
> https://guides.github.com/introduction/flow/
>
> https://zhuanlan.zhihu.com/p/23478654
>
> https://open.leancloud.cn/git-branch-guide.html





