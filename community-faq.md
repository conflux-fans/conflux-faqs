# 社区FAQ

##### 1. 普通交易的storageLimit，epochHeight 这2个字段需要关注吗？
- 调用合约时，sdk会根据cfx_estmastGasAndColletral获取storageLimit，通过cfx_getEpochNumber获取epochHeight自动设置。
- cfx转账时，storageLimit会自动填0，epochHeight会自动填当前的epoch number

##### 2. 我要一次发多笔交易，但是现在没有设置nonce的方法，这种情况怎么解决呢？
这种情况需要自己维护nonce，每笔交易nonce加一

##### 3. 0x1323ff37de4d4aa270a609dec4eef0595e7386b2在portal上导出私钥，再将其用importKey生成keystore文件，然后在节点导入keystore文件，结果地址变成了0xe323ff37de4d4aa270a609dec4eef0595e7386b2,签名的时候不影响吗？
不影响

##### 4. 发送交易返回错误 ExceedStorageLimit 什么意思?
storagelimit设置值小于实际需要的值

##### 5. 怎么把私钥，转换成keystore形式？
- go-conflux-sdk  `AccountManager.ImportKey`可以用私钥导入成keystore文件
- js-conflux-sdk `sign.encrypt`可以根据私钥生成keystore对象

##### 6. infura测试网url：
- ws://testnet.cfxchain.xyz:12535
- http://testnet.cfxchain.xyz:12537/rpc/

##### 7. 为什么节点改了配置，又要重新同步数据呢？
重启节点程序，不会从头开始同步数据，而是从数据库先恢复数据，再从最后一个checkpoint开始同步数据。因为在关闭程序时最后一个checkpoint的数据是存在内存中的，程序关闭了，内存中的数据就丢了，所以重启后看起来像是重新同步数据。

##### 8. sponsor代付功能在测试链上可以正常测试么？
可以

##### 9. CFX是erc777合约吗？
不是，cfx不是合约token，cfx相当于以太坊的eth

##### 10. Conflux支持openzepplin这个以太坊库吗？
支持，直接引用就可以，需要注意的是conflux链上erc1820合约地址与eth链不同，conflux上1820合约地址为：0x88887ed889e776bcbe2f0f9932ecfabcdfcd1820

##### 11. 谷歌插件钱包protal在哪里下载？
谷歌商店，需要翻墙

##### 12. 测试网水龙头在哪里？
- 领取cfx测试币，从portal中领取即可。
- 领取cUSDT、cDAI、cUSDC测试币，请登录https://www.ins3node.com:17005/faucet，连接考仔钱包测试币，点击request按钮即可以获取1000 cUSDT、cDAI、cUSDC测试币

##### 13. 我在用 latest_confirmed 获取epoch的时候 为什么有时候会出现下一个的值比上一次获取到的还要小？
这种在网络不好的情况下可能发生，根本原因就是区块同步延迟较高

##### 14. 一笔交易的状态待打包，已执行，这些状态是通过什么来确认的？
暂时有local rpc获取tx的pending状态 tx_inspect_pending,是获取某个address的pending的nonce范围
```
curl -X POST --data '{"jsonrpc":"2.0","method":"tx_inspect_pending","params":["0x10b719e45050518a81f56f7e7e2b1d90bdf2c228"],"id":1}' -H "Content-Type: application/json" http://127.0.0.1:12537
```
不在这个集合的就表示打包了，getTransactionReceipt的值outcomestauts不为null表示已执行。

##### 15. 开发者启动节点需要设置的地方：
- persist_tx_index = true 存储tx索引以供查询，更改该配置后需要删除所有数据重启节点，否则会卡住
- executive_trace = true 存储每笔交易执行时的函数调用栈
- enable_tracing = true 开放trace相关rpc为公开，当前只有rpc `trace_block`
- mode = "dev" 设置dev模式则不会与其他节点连接，按照设置的时间间隔出块

##### 16. cfx有查询算力的api吗？
https://www.confluxscan.io/v1/plot?interval=514&limit=10

##### 17. 如何判断发生了pivot chain switch？


##### 18. 存储抵押费是什么，怎么计算？ 比如1kb存储需要多少drip？
存储抵押指在合约新增存储占用时，需要根据新增存储抵押相应的cfx。对于每一个存储条目，最后向该条目写入的账户称为该存储条目的所有者。存储抵押费用会在该存储释放后返还给该所有者。
每1kb存储需要抵押1cfx。
> 具体请参考：https://juejin.im/post/6855551378123653127

##### 19. cfx_getTransactionReceipt返回的GasFee都包含哪些费用，包含存储抵押费用吗？
- gasfee不包含存储费，gasfee=gasUsed*gasPrice, gasfee执行完交易就花掉了，
- storageCollateralized表示实际使用的存储抵押费用，存储抵押费在存储释放的时候会返还

##### 20. 一个block中的transaction，如果其 blockHash 和 status 都是 null 的 是不是代表已经在其他的块里面处理了？
- 通常情况下是的，这是因为这个tx不是在这个block执行的，如果一个tx被重复打包，该tx会在最靠前的epoch的那个block执行。
- 还有一种情况是该transaction所在的block已打包但还没有执行。每个区块都是在打包5秒后才会执行。

##### 21. epoch 会不会出现没有block的情况？
不会，至少有一个

##### 22. js-conflux-sdk如何decode function data？
参看api文档：https://github.com/Conflux-Chain/js-conflux-sdk/blob/master/docs/api.md
```
transaction = await conflux.getTransactionByHash('0x2055f3287f1a6ce77d91f5dfdf7517a531b3a560fee1265f27dc1ff92314530b');
contract.abi.decodeData(transaction.data)
```

##### 23. 已经部署的合约可以替换吗？不产生新的合约
合约不能替换，不能升级，只能重新部署一个新的

##### 24. android和ios有对应的sdk吗？
android可以使用java-conflux-sdk, ios可以使用swift-conflux-sdk

##### 25. conflux sdk有哪些版本？
- [js-conflux-sdk](https://github.com/Conflux-Chain/js-conflux-sdk)
- [java-conflux-sdk](https://github.com/Conflux-Chain/java-conflux-sdk)
- [go-conflux-sdk](https://github.com/Conflux-Chain/go-conflux-sdk)
- [switft-conflux-wallet-sdk](https://github.com/Conflux-Chain/swift-conflux-wallet-sdk)
> python还没有

##### 26. block中的nonce跟transaction中的nonce一样吗？
transaction的nonce指from第几笔交易，block的nonce指pow计算使用的随机数

##### 27. Dapp报错 ChainIdMismatch {expected: 1029, got: 1}
这是由于Dapp需要连接主网，实际连接的是测试网

##### 28. 主网跟测试网的chaindId是多少？怎么查询
主网是1029， 测试网是1， 获取方式为rpc "cfx_getStatus" 或者使用sdk的 getStatus 方法

##### 29. conflux有随机数预言机吗？
还在与chainlink对接中

##### 30. portal不支持导入助记词? 
portal刚安装完成是可以导入助记词的，如果已经有助记词了再导入账户就只能导私钥或者keystore文件

##### 31. 怎么查看自己运行节点的bootnode数据？
- 如果是测试网或主网节点，bootnode列表就是toml配置文件里边的bootnodes配置项
- 本地节点自己就是bootnode（或者说没有bootnode）

##### 32. 合约中如何区分是在conflux链上
调用一个只读的internal contract函数，如果能成功说明在conflux上

##### 33. 交易不打包的情况有哪些？
以下条件不满足时会导致不打包：
1. balance需要满足： balance >= value + gas * gasprice + storagelimit/1024
2. nonce必须是连续的，同一账户只有当nonce低交易打包后才会打包nonce高的交易

##### 34. 怎么看每笔交易的实际扣费？
transaction receipt 的 gasFee

##### 35. confluxPortal 0.5.6 钱包账号间转cfx，几个小时了现在还在待处理，怎么办？
余额够的话，可以重置下portal，通常是 nonce 乱了

##### 36. Transaction的epochHeight和TransactionReceipt中的epochNumber一样吗？
Transaction的epochHeight用于指定tx执行的epoch范围为 (epochHeight, epochHeight + 10000), TransactionReceipt中的epochNumber指该笔交易执行时所在的epochNumber

##### 37. 怎么判断一个块是不是某个矿工挖出来的？conflux块的第一比交易也是coinbase交易，和bitcoin一样的吗？
conflux 没有coinbase 交易, block.miner 可以判断

##### 38. `0x2952a64d3afa6d39310c4928860abcd6bc097342dcc1b271b52f7809fd63f228`这笔交易在主网的显示的是合约创建，但是返回的字段contractCreated 却为null，这个时候怎么获取这个合约的地址呢?
这个是 创世块的交易，这里的 tx 比较特殊。以后的只要是合约创建 contractCreated 都会有合约的地址

##### 39. full node 跟 archive node 有什么区别？
full node只保存所有block head和最近1个era的transaction，archive node会保存所有block head和transaction

##### 40. 如何查看交易失败原因？
transaction receipt 中的 txExecErrorMsg 为交易错误信息

##### 41. tx revert 有哪些情况?
tx revert是指合约执行失败了，合约开发最好在可能发生异常的地方使用require附加错误信息，这样合约执行错误就会在transcation receipt的txExecErrorMsg中看到错误信息

##### 42. 这个错误什么意思？`Failed imported to deferred pool: Tx with same nonce already inserted. To replace it, you need to specify a gas price > 20000000000`
已经有一个同样的nonce 的tx 了，要么等他打包，要么发一个新的nonce相同的，但需要把gasPrice 提高

##### 43. js-conflux-sdk里面有没有能解析tx里面data数据的方法？
先使用abi初始化 contract，然后 contract.abi.decodeData()

##### 44. js-conflux-sdk中的Conflux类的logger除了用console，可以用别的吗？
只要实现了方法error,info,log的对象都是可以的。

##### 45. 用上代付合约，是意味着所有用户操作合约，不管是调用合约的哪个方法，都是按照一个统一的标准来支付的么？
是的

##### 46. 那一旦给合约设置了代付，是不是所有的方法都是使用代付的余额的？有方式指定某个方法跳过代付么？
当前版本不支持

##### 47. conflux代付模式有相关资料吗
https://developer.conflux-chain.org/docs/conflux-rust/internal_contract/internal_contract


##### 48. windows 10系统安装conflux-studio跑不起来
https://forum.conflux.fun/t/topic/4280

##### 49. ERC20/ERC777 在Conflux网络中还是这样称呼吗？
仍然称呼ERC20/ERC777，没有另立标准

##### 50. 主网及测试网官方节点的websocket服务端口是哪些？
主网url：  ws://main.confluxrpc.org/ws
测试网url：ws://test.confluxrpc.org/ws

##### 51. `docker pull confluxchain/conflux-rust` 时提示 "no such file" 怎么解决
可能是网络问题，可以尝试为docker设置一个registry
```
{
  "registry-mirrors": [
    "https://at8ak49f.mirror.aliyuncs.com"
  ],
  "experimental": false,
  "debug": true
}
```

##### 52. 我们在用钱包转账的时候，要求输入设置的密码，然后就转账成功了。这里为什么使用对称加密而不是私钥呢？
用户输入私钥太麻烦而且难以记忆，这个密码是 portal （钱包）的密码，私钥是存储在portal钱包里面的，本质上只有私钥才能授权发送一笔交易，密码主要是为了便于管理钱包（私钥）增加一层保护。如果对安全性要求非常高，建议使用硬件钱包。

##### 53. 区块高度是什么？
区块链是一个区块后边接着一个区块，不断的生长，区块高度指该区块到创世块的距离。

##### 54. 交易签名是什麼？
交易签名是指用私钥对交易通过签名算法得到的一个签名。验证方可以通过该签名及交易信息得到该私钥对应的地址，从而证明该笔交易确实是由该账户地址发起的。

##### 55. conflux系统中计量单位及单位转换关系？
conflux 最小单位是 drip
1cfx = 10^18 drip
1gdrip = 10^9 drip

##### 56. estimateGasAndColletral报错“Can not estimate: transaction execution failed, all gas will be charged (execution error: VmError(BadInstruction { instruction: 169 }))”
原因：部署合约时构造函数无效；通常是在调用合约忘记写to地址导致的

##### 57. 开发者如何启动一个节点
- 测试网节点：https://github.com/Conflux-Chain/conflux-rust/releases/tag/v1.1.0-testnet
- 主网节点：https://github.com/Conflux-Chain/conflux-rust/releases/tag/v1.1.0
- 私有节点：配置文件中设置 mode = "dev"

启动节点命令：`./conflux --config ./xxx.toml --full 2>stderr`

##### 58. 发送交易前需要注意什么？
发送交易记得设置 gas + strongeLimit （执行estimateGasAndCollateral可获得）， strongeLimit 是Conflux和以太坊不一样的地方。
使用sdk时，sdk在发送交易前都会自动处理。

##### 59. 区块有几种状态，顺序是什么样的？
区块有3种状态：mined, executed, confirmed
- mined就是区块被打包成功，此时区块中的交易还未执行，
- 由于mined状态时pivot block非常不稳定，避免无效执行，所以打包后5个epoch后交易才被执行，状态变为executed，
- 当区块`(confirmationRisk/2^256)<1e-8`时，状态为confirmed，此时认为该区块已确认，即从创世块到该区块所在epoch的pivot chain发生变化的概率极小(但仍有可能发生变化)