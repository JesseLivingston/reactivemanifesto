响应式宣言
----------------------

[宣言签名](http://www.reactivemanifesto.org/)

## 响应式的召唤

近年来，软件应用的需求发生了极大的变化. 几年前，一个大型的软件应用通常拥有几十台服务器，以秒计的响应时间，以小时计的停机维护时间，以及以G计的数据。而如今，软件应用被部署到几乎所有设备上，从移动设备到运行着几千个多核处理器的云集群. 用户们所要求的是以毫秒计的响应时间和100％的在线时间。数据需求更是扩展到了P级.

这些最初出现在创新互联网领域的公司如google和twitter的应用特征现在已经覆盖大多数的产业。金融和电信行业最先采用新的实践来满足新的需求，其它的行业迅速跟进。

新的需求对技术提出了新的要求。之前的解决方案着重于服务器和容器。可扩展性是通过购买更大的服务器和基于多线程的并发处理来实现的。新增的服务器的添加使用的是复杂低效昂贵的商业解决方案。

但现在已经进化出新的架构，使得开发者能够设计和创建出满足当前需求的软件应用。我们称这种应用为 *响应式应用*. 这个架构允许开发者创建 *事件驱动的, 可扩展的, 弹性的 和 交互式的* 应用: 依靠一个可扩展的可伸缩的应用栈， 适合于部署到多核和云计算的架构中，使用户获得实时的反馈，从而提升交互体验。响应式宣言所描述的正是 *实现响应式* 所必需的关键特性.

## 响应式应用

Merriam-Webster 将响应式定义为 *“随时准备响应激励”*, 即：它的组件是“活跃”的，随时准备好接收事件。这个定义抓住了响应式应用的精髓，关注系统的:

- *响应事件*: 基于事件的天性使得以下能力成为可能
- *自适应负载*: 关注可扩展性而不是单用户性能
- *自处理失败*: 创建可在所有层面上自修复的弹性系统
- *实时回应用户*: 组合以上特性而提供强交互的用户体验

以上每个特征都是响应式软件应用的核心特征。虽然在它们之间会有一定的依赖关系，但这些特性并不具备标准的分层应用架构中的那种层级关系。相反，他们所描述的设计理念贯穿整个技术栈。

![图. 1 响应式特征](images/stack.png)

下面我们将深入探讨这四种特征以及它们之间的关系。

## 事件驱动

### 为什么这是重要的

基于异步通信的应用实现了一种 *松耦合* 的设计, 它远远强于单纯基于同步方法调用的设计。发送方和接收方可以在不关心事件传送细节的情况下完成实现，使得双方的接口可以集中在通信的内容。这能够导致一种更容易扩展，进化和维护的实现，给开发人员带来更多的灵活性和更低的维护成本。

由于异步通信的接收方可以在没有事件发生或没有接收到消息时保持控制权，所以基于事件的方法可以更有效地利用现有资源，从而使数量巨大的接收者可以共享同一个硬件线程。这样，在高负载情况睛非阻塞的应用比传统的基于阻塞同步和通信原语的应用拥有 *更低的时延和更高的吞吐率*。 这将带来更低的操作成本，更高的利用率和更开心的用户。

### 关键组成部分

在基于事件的应用中，各个组件之间通过生产和消费 *事件*  — 描述事实的信息片 来进行通信这些事件以异步非阻塞的方式进行发送和接收。事件驱动的系统倾向于使用 *推（push）* 而不是*拉（pull）* 或 *轮询（poll）*, 即：他们在数据准备好后推送给消费者而不是让消费者不断地底部或等待数据从而浪费资源。

- *异步*发送事件— 也称为 *消息传递* — 意味着应用的设计本身就是高并发的，可以不修改代码而使用多核的硬件。一个CPU中的任何一个核都可以处理任何消息事件，这给并行化带来了大得多的机会。- *非阻塞* 意味着应用在硬件的使用率上非常高效，因为不活跃的组件将被挂起，它们的资源将被释放，从而能被其它组件所使用。

传统的服务端架构依赖于共享可变状态和单线程上的阻塞操作。而这两点都给对系统进行扩展和满足变化的需求造成了困难。共享可变状态需要进行同步，这引入了复杂性和不确定性，使得程序代码难以理解和维护。让线程休眠来停止使用有限的资源增加了唤醒的开销。

将事件的生成和处理分离开来可以让运行时平台来关心同步的细节以及事件在线程间的派发，这样程序的抽象级别可以提升到业务流程的层次。开发人员只需要考虑事件在系统中的传播以及组件之间的交互，而无需与低层的线程或锁原语打交道。

事件驱动的系统对组件和子系统进行了解耦。而这个级别的间接化，我们将看到，是可扩展性和弹性的前提。通过在组件之间移除复杂性和强依赖性，事件驱动的应用可以在对现有应用影响最小的条件下进行扩展。

当应用需要满足高性能和大规模可扩展性的需求时，很难预料瓶颈将出现在哪里。因此整个解决方案的异步性和非阻塞性就非常重要了。一个典型的例子是在设计时，从用户界面（浏览器、REST客户端或其它）的用户请求到Web层请求的解析和派发，再到中间件中的服务组件，一直到缓存最后到数据库，都需要是事件驱动的。如果其中一个层次不是事件驱动的 — 使用阻塞调用来访问数据库，依赖于共享可变状态、或进行了开销很大的同步操作 — 那么整个流水线就堵住了，用户将体验到变大的延时和变弱的可扩展性。

一个应用必须是 *从头到尾全面响应式* 的.

从链中去除薄弱环节的需求在 Amdahl定理中有很好的描述, 该定理在Wikipedia中描述如下:

> 使用多核处理器进行并行计算的程序的加速比受到程序中串行部分的限制。例如， 如果程序中 95% 的代码可以并行化，使用并行计算的最大加速比理论值是20倍，如图所示, 无论使用了多少处理器。

![图. 2 Amdahl定理](images/amdahl.png)

## 可扩展

### 为什么这是重要的

“可扩展性”这个词在 Merriam-Webster 中的定义是 *“可以按需进行扩充或升级的能力”*. 一个可扩展的应用有能力根据使用情况进行扩充。这可以通过在应用中添加可伸缩性来实现，从而可以按需进行向外或向内扩展（增加或减少结点）。除此之外，架构使得不用重新设计和重写应用代码就能轻松地进行向上或向下扩展（部署到拥有更多或更少核心的CPU上）。可伸缩性使得在云计算环境中操作应用的开销最小化，从而使你能从虚拟化的按使用量收费模型中获准。

可扩展性同样也对管理风险有帮助： 如果硬件太少跟不上用户负载将导致不满和客户流失，而硬件太多 — 运营团队太大 — 无理由的空闲将导致不必要的开销。可扩展的解决方案也降低了由于应用无法使用新出现的硬件产生的风险：在下一个十年中，我们将看到拥有成百上千个硬件线程的处理器出现，充分利用它们的潜力需要应用程序在很细粒度的层次上可扩展。

### 关键组成部分

基于异步消息传递的事件驱动系统提供了可扩展性的基础。它对组件和子系统进行了解耦，从而使得系统既保持同样的编程模型和主义又扩展到多个结点上成为可能。添加组件的更多实例提升了系统处理事件的能力。通过使用多核的向上扩展和通过使用数据中心或集群中的更多结点的向外扩展在实现上没有区别。应用的拓扑成为了一种部署决策，这个决策使用配置文件和/或对应用使用方法对应的自适应运行时算法来表达。这就是我们说的 [位置无关性](http://en.wikipedia.org/wiki/Location_transparency).

需要重点理解的是我们的目标并不是试图实现透明的分布式计算、分布式对象或RPC风格的通信 — 人们已经尝试过这些而且失败了。相反，我们需要在编程模型中通过异步消息传递来表现网络互联，做到 *拥抱网络* 真正的可扩展性自然包括分布式计算，对分布式计算来说，结点间的通信意味着在网络之间进行遍历，我们知道这遍历是 [天生不可靠的](http://aphyr.com/posts/288-the-network-is-reliable). 因此，就需要在编程模型中显式地表达对网络编程的限制、权衡和失败场景，而不是试图“简化”问题而将它们隐藏在有漏洞的抽象后面。这样做的结果之一是提供封装了解决分布式环境中常见问题的常用组件模块的编程工具是同样重要的 — 例如取得共识的机制或提供更高级别可靠性的消息抽象。

## 弹性

### 为什么这是重要的

应用的宕机时间是可能对商业造成最大危害的问题之一。通常它意味着运营停止了，在财务流中造成了一个空洞。从长远来看它会让用户不满并影响声誉，而这些将对商业造成严重伤害。令人吃惊的是应用程序的弹性需求要么基本被忽略要么被各种杂乱的技术给弄得面目全非。这通常意味着通常的解决方案在错误的粒度层次使用了太粗粒度的工具。一个常用的技术是使用应用服务器集群来从运行时错误和服务器故障中恢复。不幸的是，服务器故障恢复是很昂贵的，而且很危险 — 有可能导致级联的故障从而影响到整个集群。原因是这是故障管理的错误的粒度层次，在组件层次使用粒度更细的弹性管理方式能处理得更好。

Merriam-Webster对弹性的定义:

- *物品或实体还原形状的能力*
- *从困境中迅速恢复的能力*

在响应式应用中，弹性不是马后炮，而是从设计的最初就被考虑的一部分。将故障作为编程模型的一级概念提供了对故障进行响应和管理的方法，这使得应用能够在运行时对自身进行修复，从而具有高度的容错性。传统的错误处理方法无法做到这一点，因为在小的方面它太过于保守而在大的方面过于激进 — 你要么在故障发生的时间和位置对异常进行处理，要么对整个应用实例进行故障修复。

### 关键组成部分

为了 *管理故障* 我们需要能够将故障 *隔离* 出来使它不会扩散到其它组件上，, 以及能够对故障进行*观察* 使它可以被在故障环境之外的一个安全的位置进行管理。我们能想到的一个模式是 [隔板模式](http://skife.org/architecture/fault-tolerance/2009/12/31/bulkheads.html), 如图, 整个系统由不同的安全隔间构成，这样其中的某一个出现故障时其它的隔间不会受到影响。这避免了经典的[级联故障](http://en.wikipedia.org/wiki/Cascading_failure) 问题，并且允许对故障的管理也隔离完成。

![图 3 隔板](images/tank.png)

拥有可扩展性的事件驱动模型，也需要实现这种故障管理模型的原语。事件驱动模型的松耦合性为组件提供了完全的隔离，这样故障就可以与发生时的上下文一同被捕捉到，被封装成一个消息， 被发送到其它组件，由其它组件来检查错误信息并决定如何进行响应。

这种方法所创建的系统中，业务逻辑能够保持干净，从对计划外的状态的处理中分离出来，而这处理过程中，故障被明确地进行建模，从而可以被组织，观察，管理，并以描述性的方式来进行配置，系统可以自动对自身进行治愈和恢复。最好是对故障被组织成一个树形结构，很象一个大型的机构中，一个问题被向上传递直到到达一个有权力处理这个问题的级别为止。

这种模型的优美之处在于它是纯事件驱动的，基于响应式组件和异步事件，因此是 *位置无关*的. 在实际中这意味着它虽然工作在一个分布式的环境中但却拥有跟本地上下文一样的语义。

## 交互性

### 为什么这是重要的

交互式应用是实时的，好玩的，丰富的，协作性的。业务逻辑打开一个对话框来欢迎用户并引导他们进行交互体验。这使得用户更有效率，使用用户产生紧密联系、拥有能解决问题、完成任务的能力的感觉。一个例子是Google Docs，它使用户可以实时协作编辑文档 — 每个人可以随时看到其它人的编辑内容和评论。

当用户能够与实时转变成信息的数据进行交互时，它的能力被提升了。交互式应用在每一个人机界面都天生拥有这种信息的协作性，这样人们能够更有效的更频繁地进行沟通; 而这种好处随着对反馈粒度的提升（从传统的全页面刷新到每条信息的级别， 例如一个单页面的邮件web客户端）而更加突出。跨远距离的即时的社会化交互正极大的改变着人们与信息以及人们之间的关系。例如, GitHub 通过交互性的基于浏览器的“社会化编程”应用对开发者之间的协作进行了革命，当然Twitter极大地影响了新闻的传播方式。

由于构建在事件驱动的基础之上, 响应式应用拥有实现交互的有力武器。当应用变得普及时，需要良好的可扩展性来维护这个特征，需要良好的弹性来保证用户们能够持续享受这些功能。

### 关键组成部分

响应式应用使用观察者模型，事件流和有状态客户端。

观察者模型使得状态发生改变时能够让其它组件收到事件。这样就提供了从用户到系统的实时连接。例如，当多个用户同时对一个数据集进行操作时，所有的变动可以在他们之间双向地响应式地进行同步。

事件流构成了这个连接所建立的基础抽象。保持响应式意味着避免阻塞在则是允许异步的非阻塞的转换。例如一个实时数据流可能会通过一个实时的投射来生成一个新的经过分析的数据流。

大部分响应式应用拥有丰富的web界面和手机界面，给用户提供好玩的体验。. 这些应用执行逻辑并将数据存储在客户端，在客户端，观察者模型提供一个机制，当数据发生变化时，用户界面会实时改变。象WebSocket或Server-Sent Events这些技术使用户界面可以直接与事件流相连，于是事件驱动系统从服务端一直扩展到客户端。这使得响应式应用可以以可扩展和弹性的方式，使用异步和非阻塞的数据传输方法，向浏览器和手机推送事件。

了解了这一点，那么就很容易理解四种特征 *事件驱动*, *可扩展性*, *弹性* 和 *交互性* 是如何互相关联而构成一个整体的:

![图 4. 响应式特征](images/full-reactive.png)

## 结论

响应式应用代表了一种解决现代软件开发中一大类挑战的一种平衡方法。构建在 *事件驱动*, 消息传递的基础之上, 它们提供了保证 *可扩展性* 和 *弹性* 所需的工具. 而在顶端它们支持丰富的，实时的用户 *交互*. 我们预计未来的若干年中符合这种理念的系统数量将快速增长。

[宣言签名](http://www.reactivemanifesto.org/)
