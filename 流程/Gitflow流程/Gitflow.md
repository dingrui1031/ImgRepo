## 移动端Gitflow分支管理策略

### Gitflow是什么
* Git-Flow是一种代码开发合并管理流程的模式或者方法，是团队内部开发的一种约定，它并不是一成不变的，根据团队规模和现状可以动态调整。

### Gitflow工作原理
#### 用于记录历史的分支
* Gitflow使用两个分支来记录项目开发的历史，而不是使用单一的master分支。在Gitflow流程中，master只是用于保存官方的发布历史，而develop分支才是用于集成各种功能开发的分支。使用版本号为master上的所有提交打标签（tag）也很方便。  
![用于记录历史的分支](https://github.com/dingrui1031/ImgRepo/blob/main/Img/Gitflow/back_up_branch.png)

#### 用于功能开发的分支
* 每一个新功能的开发都应该各自使用独立的分支。为了备份或便于团队之间的合作，这种分支也可以被推送到中央仓库。但是，在创建新的功能开发分支时，父分支应该选择develop（而不是master）。当功能开发完成时，改动的代码应该被合并（merge）到develop分支。功能开发永远不应该直接牵扯到master。  
![用于功能开发的分支](https://github.com/dingrui1031/ImgRepo/blob/main/Img/Gitflow/dev_branch.png)

#### 用于发布的分支
* 一旦develop分支积聚了足够多的新功能（或者预定的发布日期临近了），你可以基于develop分支建立一个用于产品发布的分支。这个分支的创建意味着一个发布周期的开始，也意味着本次发布不会再增加新的功能——在这个分支上只能修复bug，做一些文档工作或者跟发布相关的任务。在一切准备就绪的时候，这个分支会被合并入master，并且用版本号打上标签。另外，发布分支上的改动还应该合并入develop分支——在发布周期内，develop分支仍然在被使用（一些开发者会把其他功能集成到develop分支）。

* 使用专门的一个分支来为发布做准备的好处是，在一个团队忙于当前的发布的同时，另一个团队可以继续为接下来的一次发布开发新功能。这也有助于清晰表明开发的状态，比如说，团队在汇报状态时可以轻松使用这样的措辞，“这星期我们要为发布4.0版本做准备。”从代码仓库的结构上也能直接反映出来。常用的一些措辞还有：基于develop新建分支，合并入master；命名规则为：release-或release/  
![用于发布的分支](https://github.com/dingrui1031/ImgRepo/blob/main/Img/Gitflow/release_branch.png)

#### 用于维护的分支
* 发布后的维护工作或者紧急问题的快速修复也需要使用一个独立的分支。这是唯一一种可以直接基于master创建的分支。一旦问题被修复了，所做的改动应该被合并入master和develop分支（或者用于当前发布的分支）。在这之后，master上还要使用更新的版本号打好标签。
* 这种为解决紧急问题专设的绿色通道，让团队不必打乱当前的工作流程，也不必等待下一次的产品发布周期。你可以把用于维护的分支看成是依附于master的一种特别的发布分支。  
![用于维护的分支](https://github.com/dingrui1031/ImgRepo/blob/main/Img/Gitflow/safeguard_branch.png)