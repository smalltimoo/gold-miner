> * 原文地址：[How to Achieve Reusability with React Components](https://medium.com/walmartlabs/how-to-achieve-reusability-with-react-components-81edeb7fb0e0#.czocsk5l0)
* 原文作者：[Alex Grigoryan](https://medium.com/@lexgrigoryan?source=post_header_lockup)
* 译文出自：[掘金翻译计划](https://github.com/xitu/gold-miner)
* 译者：[aleen42](https://github.com/aleen42)
* 校对者：[vuuihc](https://github.com/vuuihc)、[sqrthree](https://github.com/sqrthree)、[xiaoheiai4719](https://github.com/xiaoheiai4719)

# 如何实现 React 组件的可复用性 #

<img class="progressiveMedia-noscript js-progressiveMedia-inner" src="https://cdn-images-1.medium.com/max/800/1*5jIE1tOzVSuz5NPHsfeQ8w.png">

可复用性一词是当今软件工程领域上最为常见的流行词之一。可复用性早已成为大量不同框架、工具乃至模型都需要承诺的一种特性，且每一个所实现的方式与对该特性的诠释都各不相同。

### 那么，可复用性到底指的是什么？ ###

真正的可复用性指的并非是一种特定的流程，而是一个开发策略。因而，在构建可复用组件时，开发者必须得把可复用性牢记在脑海里。因为，这将涉及到无比细致的规划及善解人意的 API 设计。再者，既然可复用性早已被现代的开发工具与框架所支持且倡导，那么我们就不能仅仅通过单一的技术手段去实现该特性 —— 而需要开发团队之间的一致实现过程以及一个机构（Organizations）所有层面上的技术约定。

因此，当我们谈及可复用性时，这并不仅仅只是一场技术性的讨论。相反，它往往还会综合有公司的文化、培训以及其他很多的要素。其中部分的要素还会在本文中所触及，可关键点在于**可复用性是一个会触及到方方面面的过程，包括开发的各个阶段与一个机构中的各个层面。**

由于沃尔玛（Walmart）公司旗下包含有若干个品牌，其中包括山姆俱乐部（Sam’s Club）、阿斯达（Asda）以及一些地区分支，如沃尔玛（加拿大）与沃尔玛（巴西）等，因此，大量的前端应用会穿插在不同的品牌之间运行且由上百名开发者进行构建与维护。

也正是因为每一个品牌都会拥有着属于自己的线上品牌容貌，且开发者往往需要在一个所有沃尔玛品牌都共通的组件上工作 —— 例如，图片轮播（Image Carousel）、像面包屑（Bread Crumbs）的导航式元素、弹框和信用卡 Form 组件等。因而，往往就会存在有重复工作的现象。可众所周知的是，重复地去完成别人已完成的事情只是在浪费时间与金钱，且会增加问题所出现的机率。因此，只要能消除这样的重复性工作，开发者就能把更多的时间花费在用户体验的提升上面。

当然，也许你会说对于后端来说，在不同的品牌间共享代码会使得事情变得更为直观：即一个单一的服务器能处理来自不同品牌的多个请求，并返回对应品牌的精确数据（基于数据形式的处理方法并非只有一个）。可你是否曾想到，对于前端来说，这样的情况就会变得更为复杂。因为这将涉及到对后端所提供的数据进行提取，并把主题及其他信息准确地应用到一个特定的品牌和视图上。所以，共享代码尽管能促进组件的复用，但这并不能完全地去解决问题。

### @沃尔玛实验室（@WalmartLabs）对 React 组件的复用 ###

关于网站 Walmart.com 的构建，React 是我们当初所选择的前端框架。至于为何作出这样的抉择，其中一个原因在于其组件模型能为代码的复用提供一个好的起始点，尤其是当我们需要结合 Redux 来管理 State。尽管如此，Walmart 的体量对前端代码的复用仍然会带有显著的挑战。

### 共享代码的技术可能性 ###

共享代码所涉及的首个技术挑战是 —— 组件需要能被版本化，且易于安装及升级。对此，我们会把所有的 React 组件放置在一个分离的 GitHub 机构（Organizations）中。可目前，尽管组件已被打包放入至创建团队的仓库中，但我们仍然需要把部分的组件移至按功能分类的仓库，如“导航栏”仓库会包含有面包屑、标签及侧导航链接组件。然后，组件就会被发布至我们的私有 npm 仓库。这也就意味着开发者能非常容易地安装一个具有特定版本的组件，并保证其程序不会因版本的升级而突然抛锚。

至此，既然代码能在团队间进行共享，那么，不管组件的依赖是否更新或替换，我们都需要保证其结构与代码的一致性。这也就是为什么我们要为[组件](https://github.com/electrode-io/electrode/tree/master/packages/electrode-archetype-react-component)与[应用](https://github.com/electrode-io/electrode/tree/master/packages/electrode-archetype-react-app)创造出 [Electrode 原型](http://www.electrode.io/docs/what_are_archetypes.html)。该原型不仅包含有用于代码规范、转译及封装的配置文件，而且还提供有用于管理核心代码依赖与任务/脚本的核心。这样一来，从一个通用的结构开始，建立起项目间一致的代码标准就能使得机构能维持有最好的现代化实践，且能同时增强开发者间的编程信心，与提高可复用组件所真正被复用的机会。此外，一个包含有代码规范、性能估算与多设备、多平台及多分辨率测试的稳定持续集成/持续部署（Continuous Integration/Continuous Deployment）系统同样会起到进一步的促进效果。详细来说，持续集成系统会在 PR 请求提交时包含有所有的规则，并发布一个 beta 测试版本，以供所有相关程序测试。这样的话，就能保证此次 PR 不会影响到任何的地方。

### 元队（The Meta Team） ###

在项目初期，由于大部分的共享组件是由少量的团队所贡献完成的，因而它们更新迭代的速度会变得越来越快。从而最终导致我们不得不选择少部分对 Electrode 原型与沃尔玛内部具有深入了解的开发者，来创造出我们所称作的“元队”。而被选中的人会在数周内抽出若干小时乃至一整天的时间去对机构中正在运行的组件代码进行审查，以确保开发者能遵循实践，并尽可能地协助他们去解决问题。此外，该团队还会就机构中所构建的东西总结出一整套知识体系，并充当着使者的角色去为自己团队中采用 [Electrode](http://www.electrode.io/) 原型的项目提供服务。而且，元队成员还会把关于原型修改待决的部分信息带至自己的团队中，以收集回馈并与 Electrode 原型的核心开发团队进行分享和探讨。

尽管，好的开始是成功的一半。但作为一个机构，我们仍然能看到代码复用更进一步的提升空间。

### 上百个组件的暴露性问题 ###

随着共享主题的策略实施，我们开始注意到 Slack 频道上所涌现出来的大量信息。开发者希望能有一种方式去得知是否已经有现有的组件能完成一个特定的任务，UX 团队希望能查看到哪些组件是可用的，而项目经理则希望能查看到哪些组件是其他团队正在构建中的。也就是说，所有这些信息所围绕的共同焦点就在于组件是否能被暴露。针对于此，我们迫切需要一种快速且简单的方法，去发现可用的组件并查看它们的使用情况。从而能与这些组件进行交互，以了解它们的实现、配置及依赖。

那么，问题答案就在于[我过去曾写文讨论的一样东西 —— Electrode 勘探器](https://medium.com/walmartlabs/spotlight-on-electrode-explorer-react-component-reuse-without-the-hassle-6447763365b2#.etp9o5wr0)。开发者通过该勘探器不仅能浏览到@沃尔玛实验室中上百个可用的组件及其文档，而且还能浏览到组件的版本提交记录，以查看其各阶段的代码修改情况。正是因为 Electrode 勘探器能提供机构中所有组件的 Web 接口，因而开发者此时已不再需要键入 `npm install` 来查看并使用组件。

### 缝隙间所溢出的重复组件 ###

尽管，所有的这些工具与工作流程都是在促进代码的复用，但问题依旧存在。而其中一点就是，开发团队在开发新组件时往往会忽略掉组件对其他团队所起到的作用。倘若组件没有被涵盖在可复用的生态系统当中，那么，这就意味着该组件无法被其他人复用。即便是存在于同一套共享组件系统，大量重复或一些对相似问题采用不同解决办法的组件依旧存在。因而，我们这才意识到技术手段并非能完全地解决问题。所以，此时此刻我们需要的是一种办法。它不仅能广泛地改变公司人员的思考方式，而且还能使得所有层面的工作人员都能事事以可复用性当先。这也就包括了花费时间去对之前的组件进行归纳总结，以便复用变得更为简单；在已有组件的基础上进行扩展，而不是从零开始；不断地去寻找机会来尽可能地与外界共享代码。

为了协助思想上的这种改变，我们创建了一套组件开发的提案流程。在此系统下，开发者需要在工作开始前先讨论关于新组件的一切事宜，以便机构中的其他团队能根据此事来推荐出已有的解决方案或可选方法。这样，机构中的其他人也就能知道发生着怎样的事情。

> **实践证明，在开发过程当中采用该种提案系统能有效地帮助我们解决缝隙间所溢出的重复组件问题。**

### 持续集成/持续部署系统的重要性 ###

过去，我们曾遇到过这样的一个重大的问题：一个团队在组件上的开发可能会导致其他团队的程序抛锚。换而言之，如果你不对组件的版本进行锁定，持续集成/持续部署系统可能会因为组件被其他团队所修改，而导致出错 —— 这是一个非常糟糕的感觉。甚者还会到导致大量团队需要封锁自身组件至一个特定的版本，而无法使用新的补丁版本。

这也就是为何我们需要引入一个稳定的持续集成/持续部署系统。当一个组件版本更新时，不管其他重要的程序是否对该组件的版本进行锁定，系统中的自动装置都会去检查本次更新是否会导致程序主版本的崩溃。若无，则生成一个 PR 请求去更新锁定的版本至最新版本号；而若有，则通知涉事团队双方去检讨问题的所在。

### 内部资源 ###

关于提高 React 组件可复用性的这些方法都是基于 [Laurent Desegur](https://twitter.com/ldesegur) 早前[写文](https://medium.com/walmartlabs/beyond-open-source-walmartlabs-e690c934fe35#.lqc0e6x3b)所描述的关于开放资源/内部资源哲学思想的一些领会。随着像 Hapi、[OneOps](https://github.com/oneops) 与 [Electrode](https://github.com/electrode-io) 等一些项目的展现，可以看到@沃尔玛实验室在过去几年已逐渐成为开源征途上的使用者及贡献者。尽管，在公司外面的人看来，我们对内部资源看似甚少贡献，即那些基于开源模型所开发的内部程序。但是，对于内部资源来说，并没有任何一个团队或成员会真正地“拥有”一个组件。换句话说，所有的组件是共享在整个机构当中的，而这也就意味着能消除开发的瓶颈并驱使开发者去提升已有组件的质量。

采用这样的策略不仅能很好地提高组件复用的机率，而且更为重要的是，还能为我们的开发者及开发协作的哲学思想提供有一定指引。而且，策略一定程度上能驱使开发者把自己的时间和专业知识使用在最需要的地方，而不是静待技术瓶颈的消除。这样的话，他们就能以真实且可量化的方式让公司受益。

### 总结 ###

可复用性不仅是一种技术性的决策，而且还是一种需要机构性保障，且具有深远意义的哲学思想。通过@沃尔玛实验室这个例子，我们就可以清晰地看到其所产生的效益是何等巨大。如今，开发者们也正在把 SamsClub.com 移植到 [Electrode 平台](https://github.com/electrode-io)上，并复用数百个来自 Walmart.com 的组件去匹配山姆俱乐部的品牌容貌。

当然，最后你也可以跟我们分享一下自己关于可复用性的一些故事，包括当中遇到了哪些阻碍？怎么去解决？以及你所能看到的哪些更深层次的提升？
