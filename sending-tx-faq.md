<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [如何发送交易?](#%E5%A6%82%E4%BD%95%E5%8F%91%E9%80%81%E4%BA%A4%E6%98%93)
- [发送交易时的 `燃气费`，`存储费` 指的是什么？](#%E5%8F%91%E9%80%81%E4%BA%A4%E6%98%93%E6%97%B6%E7%9A%84-%E7%87%83%E6%B0%94%E8%B4%B9%E5%AD%98%E5%82%A8%E8%B4%B9-%E6%8C%87%E7%9A%84%E6%98%AF%E4%BB%80%E4%B9%88)
- [使用 SDK 发送交易，需要指定什么信息（参数）](#%E4%BD%BF%E7%94%A8-sdk-%E5%8F%91%E9%80%81%E4%BA%A4%E6%98%93%E9%9C%80%E8%A6%81%E6%8C%87%E5%AE%9A%E4%BB%80%E4%B9%88%E4%BF%A1%E6%81%AF%E5%8F%82%E6%95%B0)
- [除了 from, to, value 一笔 TX 还包含哪些信息?](#%E9%99%A4%E4%BA%86-from-to-value-%E4%B8%80%E7%AC%94-tx-%E8%BF%98%E5%8C%85%E5%90%AB%E5%93%AA%E4%BA%9B%E4%BF%A1%E6%81%AF)
- [如何获取正确的 nonce?](#%E5%A6%82%E4%BD%95%E8%8E%B7%E5%8F%96%E6%AD%A3%E7%A1%AE%E7%9A%84-nonce)
- [nonce 什么时候回加 1？ 交易执行失败 nonce 会加 1 么？为什么交易发送了，nonce 没有变化？](#nonce-%E4%BB%80%E4%B9%88%E6%97%B6%E5%80%99%E5%9B%9E%E5%8A%A0-1-%E4%BA%A4%E6%98%93%E6%89%A7%E8%A1%8C%E5%A4%B1%E8%B4%A5-nonce-%E4%BC%9A%E5%8A%A0-1-%E4%B9%88%E4%B8%BA%E4%BB%80%E4%B9%88%E4%BA%A4%E6%98%93%E5%8F%91%E9%80%81%E4%BA%86nonce-%E6%B2%A1%E6%9C%89%E5%8F%98%E5%8C%96)
- [交易实际使用的燃气费如何计算 ？](#%E4%BA%A4%E6%98%93%E5%AE%9E%E9%99%85%E4%BD%BF%E7%94%A8%E7%9A%84%E7%87%83%E6%B0%94%E8%B4%B9%E5%A6%82%E4%BD%95%E8%AE%A1%E7%AE%97-)
- [如何释放存储抵押 ？](#%E5%A6%82%E4%BD%95%E9%87%8A%E6%94%BE%E5%AD%98%E5%82%A8%E6%8A%B5%E6%8A%BC-)
- [为什么跟合约交互，交易消耗了燃气费，但我的余额没有变？](#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%B7%9F%E5%90%88%E7%BA%A6%E4%BA%A4%E4%BA%92%E4%BA%A4%E6%98%93%E6%B6%88%E8%80%97%E4%BA%86%E7%87%83%E6%B0%94%E8%B4%B9%E4%BD%86%E6%88%91%E7%9A%84%E4%BD%99%E9%A2%9D%E6%B2%A1%E6%9C%89%E5%8F%98)
- [如果要批量发送交易，如何管理 nonce ？](#%E5%A6%82%E6%9E%9C%E8%A6%81%E6%89%B9%E9%87%8F%E5%8F%91%E9%80%81%E4%BA%A4%E6%98%93%E5%A6%82%E4%BD%95%E7%AE%A1%E7%90%86-nonce-)
- [如何知道一笔交易所会使用的 gas 和 storage ?](#%E5%A6%82%E4%BD%95%E7%9F%A5%E9%81%93%E4%B8%80%E7%AC%94%E4%BA%A4%E6%98%93%E6%89%80%E4%BC%9A%E4%BD%BF%E7%94%A8%E7%9A%84-gas-%E5%92%8C-storage-)
- [怎么知道一笔交易执行成功了 ？](#%E6%80%8E%E4%B9%88%E7%9F%A5%E9%81%93%E4%B8%80%E7%AC%94%E4%BA%A4%E6%98%93%E6%89%A7%E8%A1%8C%E6%88%90%E5%8A%9F%E4%BA%86-)
- [怎么判断一笔交易是否安全，是否被确认?](#%E6%80%8E%E4%B9%88%E5%88%A4%E6%96%AD%E4%B8%80%E7%AC%94%E4%BA%A4%E6%98%93%E6%98%AF%E5%90%A6%E5%AE%89%E5%85%A8%E6%98%AF%E5%90%A6%E8%A2%AB%E7%A1%AE%E8%AE%A4)
- [`receipt` 是什么, 都包含什么信息 ?](#receipt-%E6%98%AF%E4%BB%80%E4%B9%88-%E9%83%BD%E5%8C%85%E5%90%AB%E4%BB%80%E4%B9%88%E4%BF%A1%E6%81%AF-)
- [交易的状态是如何发生变化的？](#%E4%BA%A4%E6%98%93%E7%9A%84%E7%8A%B6%E6%80%81%E6%98%AF%E5%A6%82%E4%BD%95%E5%8F%91%E7%94%9F%E5%8F%98%E5%8C%96%E7%9A%84)
- [为什么交易发送失败 ？](#%E4%B8%BA%E4%BB%80%E4%B9%88%E4%BA%A4%E6%98%93%E5%8F%91%E9%80%81%E5%A4%B1%E8%B4%A5-)
- [为什么交易一直不被打包 ？](#%E4%B8%BA%E4%BB%80%E4%B9%88%E4%BA%A4%E6%98%93%E4%B8%80%E7%9B%B4%E4%B8%8D%E8%A2%AB%E6%89%93%E5%8C%85-)
- [为什么交易执行失败 ？](#%E4%B8%BA%E4%BB%80%E4%B9%88%E4%BA%A4%E6%98%93%E6%89%A7%E8%A1%8C%E5%A4%B1%E8%B4%A5-)
- [发送交易的常见错误有哪些?](#%E5%8F%91%E9%80%81%E4%BA%A4%E6%98%93%E7%9A%84%E5%B8%B8%E8%A7%81%E9%94%99%E8%AF%AF%E6%9C%89%E5%93%AA%E4%BA%9B)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



### 如何发送交易?
发送交易最简单的方式是使用钱包比如：Conflux Portal，DappBirds 等，点击发送直接设置金额即可。
如果你是一个开发者，你可以使用 Conflux SDK（JS，Java，Go） 自行构建交易，然后通过 node RPC 发送到链上。


### 发送交易时的 `燃气费`，`存储费` 指的是什么？
燃气费是交易执行的手续费, 矿工打包并执行交易需要收取一定的费用，其计算方式为 gasFee = gasPrice * gasUsed。
交易的执行可能会占用新的存储空间，此部分空间的占用不需要支付费用，但需要为存储的使用抵押一部分 CFX，随着存储的释放，抵押也会归还。
存储费指的的存储占用抵押的 CFX，每占用 1024 字节需要抵押 1 CFX。


### 使用 SDK 发送交易，需要指定什么信息（参数）
使用 JS-SDK 发送交易，如果只进行简单的 CFX 转账，只需指定 `from`(从哪个账户转出), `to`（发到哪个账户）, `value`（发送数量，单位是Drip）即可。


### 除了 from, to, value 一笔 TX 还包含哪些信息?

* `from`: 交易的发出方地址，不能为空，且只能是普通账户地址，不能是合约
* `to`: 交易的接收方，可以为空, 如果为空，则会使用 data 信息创建智能合约，新创建合约的地址可以在 tx receipt 中获取
* `value`: 交易的金额，单位为 Drip
* `nonce`: 交易执行的序号，通常该续号从0开始，每发送一笔交易加 1，交易的执行也会按照递增的顺序执行，不会跳跃，cfx_getNextNonce 可以获取某地址的下一个 nonce
* `gas`: 交易执行所能消耗的燃气费数量上限，
* `gasPrice`: 交易燃气费的价格，单位为 Drip
* `storageLimit`: 交易执行所能支付的存储最大上限
* `chainId`: 链的ID，用于区别不同链的交易，主网-1029, 测试网-1
* `epochHeight`: 用于指定tx执行的epoch范围为 (epochHeight, epochHeight + 10000), 超过该范围交易将不会被打包
* `data`: 交易的数据，可以是备注，也可以是合约创建 ByteCode，或者是合约交互的 data


### 如何获取正确的 nonce?
通过RPC `cfx_getNextNonce` 可以获取某个账户的下一个可用的nonce, 使用过的nonce 无法再次使用， 如果使用一个比当前nonce 大的值，则交易不会被打包

### nonce 什么时候回加 1？ 交易执行失败 nonce 会加 1 么？为什么交易发送了，nonce 没有变化？

### 交易实际使用的燃气费如何计算 ？
在 scan 的交易详情页可以看到 gas 的使用量，gas 价格，gas 费用等信息，其本质是通过 `cfx_getTransactionReceipt` 获取的
gasFee = gasCharged * gasPrice, 但 gasCharged 不一定和 gasUsed 相等
在 conflux 有一个规则，gas 用于指定一笔交易所能使用的燃气上限，一定会大于燃气的实际使用值(gasUsed), 对于多的部分，最多只会回退 1/4: 如果多的部分小于gasLimit 的 1/4 全部退回，但如果大于 1/4 
则只回退 1/4, 因此发送交易时尽量指定准确的 `gas` 值 


### 如何释放存储抵押 ？
释放存储则抵押自动被回退，比如ERC20 token 的余额从非 0变为 0，则会回退抵押，比如销毁某个合约，则会回退所有地址跟合约交互所产生的抵押

### 为什么跟合约交互，交易消耗了燃气费，但我的余额没有变？
Conflux 网络有赞助者机制，如果某个合约有赞助者，则跟合约交互的燃气存储费用由赞助者出

### 如果要批量发送交易，如何管理 nonce ？
批量发送交易时，需要手动管理 nonce，每发送一笔交易，nonce 手动加一，此种情况，对于失败并且nonce未使用的交易，需要手动调整 tx 参数重新发送.
因此批量发送交易时需要保留所有 tx 的 hash 并自动监控交易的状态

### 如何知道一笔交易所会使用的 gas 和 storage ?
RPC `cfx_estimateGasAndCollateral` 可以用于预估一笔交易所需要使用的 gas 和 storage 数量，但预估结果并非 100% 准确，对于返回的 gas 可以相应手动调大一些比如乘上 `1.3` 

### 怎么知道一笔交易执行成功了 ？
查看交易 的 `status` 字段 或 receipt 的 `outcomeStatus` 字段可以判断交易是否成功，0 表示成功 1 表示失败

### 怎么判断一笔交易是否安全，是否被确认?
如果一笔交易所在 epoch 的 epochNumber 小于当前的 confirmed epochNuber 则认为安全了.
也可以通过 RPC `cfx_getConfirmationRiskByHash` 获取交易所在 block 的 confirmationRisk，如果得到的值小于 1e-8 则认为是安全的

### `receipt` 是什么, 都包含什么信息 ?
receipt 是交易的回执信息，直译过来是`票据`, 通过 receipt 可以知道交易执行的一些情况比如是否成功，是否创建合约，gas 花费情况，tx 执行产生的 eventLog等
具体[参看](https://developer.conflux-chain.org/docs/conflux-doc/docs/json_rpc#cfx_gettransactionreceipt)

### 交易的状态是如何发生变化的？
交易通过 RPC 提交之后会经历几个状态： 等待打包->打包->执行->确认 [具体参看](https://developer.conflux-chain.org/docs/conflux-doc/docs/send_transaction#track-my-transaction)

### 为什么交易发送失败 ？

### 为什么交易一直不被打包 ？

### 为什么交易执行失败 ？


### 发送交易的常见错误有哪些?

1. code = -32015, message = Estimation isn't accurate: transaction is reverted. Execution output , data =0xxxx