### 《Conflux 协议规范》中有大量的 Conflux 协议细节，这使得它不易读，对于想大致了解 Conflux 技术架构的读者也不够友好。有没有相关解读文章？
[Conflux 研究院 | 《Conflux 协议规范》（黄皮书）导读](https://juejin.cn/post/6896346384018964493)


### Conflux 相比以太坊有什么优势？
TPS高，手续费低，支持代付，兼容 Solidity


### Conflux 代币是什么，最小单位是多少？
Conflux 的原生代币是 CFX, 最小单位为 Drip  1CFX=10e18 Drip


### Conflux 是 pow 还是 pos， 挖一块奖励几个CFX？
POW, 单个区块的奖励为 2CFX


### FC 跟 CFX 的关系


### Epoch 是什么，Epoch 跟 block 的关系是怎样的？


### Conflux 的 TPS 能达到多少
3000-6000

### Conflux 是什么语言开发的？
Rust


### Conflux 的官方钱包是什么？
Portal 同 MetaMask 类似是一款浏览器插件钱包


### Conflux 账户地址跟以太坊是否兼容？
不兼容：Conflux 地址长度跟以太坊相同，总体有三类地址：0x1 开头普通地址，0x8 开头合约地址，0x0开头内置合约地址
以太坊则没有此区分

### Conflux 使用什么语言开发智能合约?
Solidity

### Conflux 为什么需要延迟执行？

<br>
<h3 id="1">发送交易，交易额，低于多少，就显示成0了呢？</h3>

只有等于0会显示为0, 哪怕是1 Drip 也会显示.
<br>

<h3 id="2">这个gas费是什么意思？所支付的gas费最终去了哪里？为什么有的转账所需要的费用特别多，有的少，多出来的去了哪里？</h3>

发送交易需要矿工打包执行，gas 费是付给矿工的，这样才会有人愿意运行节点，gas 支付多少根发送交易的 复杂度，交易期望执行时间有关。多出来的一般会退回，不同的链具体回退机制也不一样。

Conflux挖矿奖励有3部分

1.出块基础奖励

2.手续费（gas费）

3.抵押存储产生的利息

portal默认的gas price只有1Drip，1cfx=100000000000000000drip
<br>

<h3 id="3">测试网水龙头地址</h3>

水龙头在 portal 的存入里面,网络需要先切为测试网。
<br>

