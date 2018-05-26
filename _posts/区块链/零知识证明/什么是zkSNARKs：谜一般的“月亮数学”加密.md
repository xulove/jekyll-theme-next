## 什么是zkSNARKs：谜一般的“月亮数学”加密

随着以太坊进入“大都会（Metropolis）”阶段，全方位的升级使得隐私得到更好的保障，当然这也让以太坊更加复杂抽象。其中一项升级，引入了“Zero-Knowledge Succinct Non-Interactive Argument of Knowledge（零知识下简明的非交互知识论证，非常拗口......）”，简称为 **Zk-Snarks**。Zk-Snarks 的运用基于零知识证明的思想，在本文中，我们会带着大家一起探索零知识证明概念，以及它在区块链中的相关应用。

![img](https://mmbiz.qpic.cn/mmbiz_png/Xy3fkoVasibpkaa6bFibjB9TqvXu3jX44AHLsJkES09Ve4ktDf5ul4H5dZ6UVBBMDvBd7a9TkqqrFSQA5RKpZplw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

零知识证明出现于20世纪80年代左右，这得感谢三位麻省理工学院研究员，Shafi Goldwasser、Silvio Micali 和 Charles Rackoff 的工作。当时这些人在研究交互证明系统相关的问题——即一种理论系统：证明者（Prover）可以和验证者（Verifier）（下文会展开说明）交换信息，在不直接展示该知识的情况下，使验证者确信“**证明者的确拥有这项知识**”。

在他们三位提出这项划时代的发现之前，大多数的证明系统主要聚焦在系统本身的可靠性（Soundness）。原先大家都假设，在任何场景下证明者都可能是恶意的一方，并试图误导验证者。但 Goldwasser 等人却从另一个角度着眼，提出对于验证者的道德质疑。他们的质疑是：我们如何知道在验证过程中，验证者不会泄露任何知识？以及在一次次验证中，验证者是否会从证明者手中从中获得一定量的知识？

在现实生活中，有许多问题也因此而产生。其中最为人所知的，就是密码保护问题。假设你想要使用密码登录网站，标准化的协议流程是这样的：客户端（你）写下密码并发送给服务器，服务器将你的密码进行哈希运算，然后并对比存储在服务器端的密码哈希值。如果两者相同，你便能登录系统。

你发现这里有个巨大的缺陷了吗？

上述过程中，服务器需要拥有你的密码明文，所以你的隐私能否被保障就全看服务器端（也就是本场景下的验证者）的脸色。如果服务器受到攻击，甚至被入侵，那么你的密码就会暴露给恶意攻击者，导致严重的后果。为了应对这种问题，零知识证明在每个场景都有其创新性及必要性。

只要谈及零知识证明（如上所述），就要提到两个角色：证明者（Prover）和验证者（Verifier）。零知识证明核心是：证明者可以在不告诉验证者某知识是什么的情况下，使得验证者相信他们的确掌握该知识。

### 

### **零知识证明的特性**

零知识证明需要满足以下特性：

- **完整性（Completeness）**：如果论述（校注：这里的“论述”即“零知识证明”中的“知识”）为真，那么诚实的证明者一定能说服诚实的验证者。
- **可靠性（Soundness）**：如果证明者不诚实，他们无法通过造假来说服验证者接受某论述。
- **零知识性（Zero-Knowledge）**：如果论述为真，验证者无法得知论述实际内容是什么。

现在我们对零知识证明有了基本概念，在深入探讨 zk-snarks 及其在区块链中的应用之前，让我们先来看几个例子。

### **案例1 阿里巴巴的洞穴**

在这个例子里，证明者(P) 向验证者(V) 宣称：他们知道洞穴后面密门的密码，而且希望在不透露密码的情况下，向验证者(V) 证明这件事。

所以情况如下图：

![img](https://mmbiz.qpic.cn/mmbiz_png/Xy3fkoVasibpkaa6bFibjB9TqvXu3jX44AyGo6mbZUnibr3kP3MKqVX4wFPiax7uHIxwqfs8vJwTRm1DeYoZF4mtibQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)*-图片来源：Scott Twombly（YouTube频道）-*

证明者沿着 A 或 B 任一条路径走进去。假设他们一开始选择 A 路径，然后到达后方的密门。接着，验证者来到洞口，并要求看到证明者从 B 路径出现。（请注意，这时候验证者并不知道证明者选择哪一条路径走进洞穴。）

正如你从图中所见，证明者的确从路径 B 出现。但这会不会只是运气好？如果证明者不知道密门密码，却恰好选择路径 B 走入洞穴，而验证者又凑巧要求他从路径B出现，这时虽然验证者被卡在密门前，仍然可以通过测试。

所以为了保证验证有效，必须进行多次。如果证明者每一次都能出现在正确的路径上，那么证明者就能向验证者证明他的确知道密门密码，同时验证者无法得知密码到底是什么。

**我们来看看在这个例子里，零知识证明的三个特性是如何满足的：**

- **完整性（Completeness）**：因为该论述是真实的，诚实的证明者(P) 可以说服诚实的验证者(V)。
- **可靠性（Soundness）**：如果证明者(P) 不诚实（译注：即其实他们不知道密码），他们也无法在多次实验下欺骗验证者(V)。即便证明者(P) 很幸运，运气也总有耗尽的一天。
- **零知识性（Zero-Knowledge）**：验证者(V) 自始至终都不知道密码是什么，但他会被说服证明者(P) 拥有密码。

### **案例2 寻找瓦尔多**

还记得“寻找瓦尔多”游戏吗？

当然，你肯定还记得！不管是在现实生活中或是网上，你一定曾经见过这款游戏。给不知道的人做个说明，“寻找瓦尔多”是一款游戏，任务是从茫茫人海中，找到“瓦尔多”在哪里，是个单纯的寻人游戏。这个游戏看起来如下图：

![img](https://mmbiz.qpic.cn/mmbiz_jpg/Xy3fkoVasibpkaa6bFibjB9TqvXu3jX44AdqPmkPGA2oTWOdibHp7d7JAHOVricoEfEY3fjsj70RDibAHOjAqib6kYoQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)*-图片来源：Youtube (IntoConnection)-*

目标就是找到下图这个名叫“瓦尔多”的人：

![img](https://mmbiz.qpic.cn/mmbiz_jpg/Xy3fkoVasibpkaa6bFibjB9TqvXu3jX44AhftMbkHzhQROicojNy6A7vhRX2KVbO6k30pkecxibzyaYoarbaOK9WuQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)*-图片来源：Pinterest-*

看起来非常简单对吧！从图片中的人海里找出瓦尔多。那么零知识的概念又在哪儿出现呢？发挥一下你的想象，假设现在有两个玩家 Anna（女）和 Carl（男）。Anna 告诉 Carl，她已经找到瓦尔多，但她不想告诉 Carl 瓦尔多的具体位置。现在，Anna 如何在不指出瓦尔多位置的情况下，向 Carl 证明她的确知道正确位置了？

这引出了两种零知识证明解决办法，分别是“中级解决办法”和“简易解决办法”，由 Naor 和 Reingold 在一篇有趣的论文中所提出。接下来让我们娓娓道来。

中级解决办法

之所以称这个解决办法为“中级的”，是因为证明者(Anna) 和验证者(Carl) 需要使用复印机来完成这事。整个流程是这样的：首先，Anna 和 Carl 将游戏原图复印，接着 Anna 确保 Carl 没有偷看，并把复印件上的瓦尔多剪下来，然后将复印件剩余部分扔掉。这样一来，她就可以向Carl展示从复印件剪下来的瓦尔多图片，以此证明她的确知道游戏里瓦尔多的位置，同时不需要指出来。

这个方案会碰到问题：虽然上述手段满足了“零知识性”，却没有保证“可靠性”，也就是说 Anna 仍然可以在这个方案中欺骗 Carl。她只要在一开始手上就拿着任意一张瓦尔多裁切图，即使她不知道游戏里瓦尔多在哪，仍然能向 Carl 展示事先准备好的瓦尔多裁切图，让 Carl 误以为她知道正确位置。

因此我们需要更细致而谨慎的测试。首先，Anna 和 Carl 将游戏图复印，然后由 Carl 在复印件背面画上特殊的图案。接着，Carl 将 Anna 带到一间隔离的房间，并确保无论如何她都不会有作弊的机会。如果 Anna 在这种情况下仍然能拿出瓦尔多裁切图，Carl 就会相信她的确知道游戏里瓦尔多的正确位置。他们可以多次重复实验，同时 Carl 可以反复对比确认这几次的瓦尔多裁切图，以确保 Anna 提供的裁切图有效。

简易解决办法

这个方法非常简单，只需要非常基础的工具。首先我们拿来一个大纸板，大约是游戏图片的两倍大小，然后在纸板上剪出一个矩形小窗口。接着，Anna 可以在确保 Carl 没有偷看的时候，在游戏图上移动纸板，使得瓦尔多的图案正好出现在矩形窗口中。然后 Anna 告诉 Carl 可以睁眼瞧瞧，下面是 Carl 看到的样子：

![img](https://mmbiz.qpic.cn/mmbiz_png/Xy3fkoVasibpkaa6bFibjB9TqvXu3jX44ARIgQSsPGHGmoWJNzqVhpk43f0Tv5T3qvI42ODnnXibXuz7Z8dCnGRzg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

*-图片来源：Naor 和 Reingold 编写的《给孩子看的应用密码学》（Applied Kid Cryptography）-*

虽然 Carl 可能会猜到瓦尔多可能在哪，但他实际上无法得知具体位置。Anna 因而在没有指出瓦尔多位置的情况下，向 Carl 证明她其实知道这个答案。

### **案例3 数独**

另一个使用零知识证明的绝佳例子是日本的著名谜题，数独。首先玩家会得到一个如下的 9x9 表格：

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)*-图片来源：Computational Complexity Blog-*

这个游戏的目标是使用 1~9 的数字填满每一行、每一列，及每个3x3的区块，其中数字 1~9 不能重复。前面数独谜题的解答如下图所示：

![img](https://mmbiz.qpic.cn/mmbiz_png/Xy3fkoVasibpkaa6bFibjB9TqvXu3jX44AyKWurqibnzaLr2yuMcuwxCJXjibZgzmkPic2Hv1oln5syhYA2I5A54JMQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)*-图片来源：Computational Complexity Blog-*

如你所见，每一行、每一列，及每个 3x3 区块里数字都是唯一的，没有数字重复。接着请出我们的老朋友 Anna 和 Carl。现在 Anna 已经找到数独的解，但多疑的 Carl 就是不信，他希望 Anna 能够证明自己的确知道数独的解。Anna 要证明自己真的知道，但与此同时，她不希望 Carl 得到这个数独的解。她该怎么做呢？Anna 将使用零知识证明来佐证她的说辞。

首先，Carl 要使用已经被验证过的一个诚实的计算机程序运行该数独谜题，这个程序会**随机选择**每个数字的替代码。比如对于这个特定的数独谜题，程序选择了以下替代码：

![img](https://mmbiz.qpic.cn/mmbiz_png/Xy3fkoVasibpkaa6bFibjB9TqvXu3jX44AoqzibJ8D1w2EAH2vWHwZjqQxMyiaCvJibia7wRxfgFmaYeLmiaxb9cnAhKA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

程序选中的替代码使得每个 数字 都有同样的机会被转换为其他数字。基本上，数字 1 和数字 3 被替换可能性是一样的，数字 4 和数字 9 也是如此。因此，根据上面的替代码，我们替换一下数独谜题的解：

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

*-图片来源：Computational Complexity Blog-*

现在 Anna 拿着这份密码替换表。请注意，此时 Carl 并不知道初始数独谜题的解是什么，也不知道数字替换表的内容。接着，Anna 利用“锁箱机制（lockbox mechanism）”隐藏谜题中的所有数字，因此 Carl 只能看到一个空白的 9x9 表格。

现在 Carl 有28种选择：

- 显示任一行 （9行）
- 显示任一列（9列）
- 显示任一 3x3 区块（9块）
- 显示初始谜题的替换后版本

假设 Carl 选择显示第三列，如下图所示：

![img](https://mmbiz.qpic.cn/mmbiz_png/Xy3fkoVasibpkaa6bFibjB9TqvXu3jX44AP3OMI5QmTyepCJqYdrZgTeesCXjQECUaETibUszk3QIp2hQCjibvmWbg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

*-图片来源：Computational Complexity Blog-*

Carl 将会看到这样的画面：每个数字在该列中都是唯一出现，而上初始谜题中的每个数字被替换的概率相同，因此 Carl 无法推出初始谜题的解是什么。

现在假设，Carl 决定选择最后一项，他想看到初始谜题的替换后版本的模样，他会看到如下图所示：

![img](https://mmbiz.qpic.cn/mmbiz_png/Xy3fkoVasibpkaa6bFibjB9TqvXu3jX44AIHgqwAajhbuLEODM0DmgD3E1URhrc0OhFneHGkShJrXZ3GIxWgklrA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

*-图片来源：Computational Complexity Blog-*

同样的，因为替代码是随机选择的，每个数字都有相同的替换概率，Carl 将无法得知初始谜题的解。Carl 每回都可以从这 28 种选择中则以进行验证，最终他会相信 Anna 的论述（Anna 说自己知道数独谜题的解）

为什么呢？

因为如果 Anna 的确作弊（她不知道初始谜题的解），她就无法找到完整的替换码，使得 Carl 的 28 种选择都有唯一的解。假如 Carl 只验证了一次，比如他就看了其中一行，那么 Anna 作弊成功的概率高达 27/28。但如果 Carl 进行多达 150 次验证，那 Anna 作弊成功的概率就会降至 (27/28)^150，作弊成功率连0.5%都不到。（编者注：所谓进行 150 次验证的意思应该是更换 150 次密码替换表，每一回验证都让 Carl 选择 28 种选择中的 1 个。）

接着让我们检查一下，零知识证明特性在这个案例中是否满足：

- **完整性（Completeness）**：密码程序经过验证所以可信，而且 Anna 和 Carl 都接受这个协议。
- **可靠性（Soundness）**：如果做了150次随机测试，那么 Anna 作弊成功的概率小于0.5%。
- **零知识性（Zero-Knowledge）**：Anna 从头到尾都不必向 Carl 透露初始谜题的解。