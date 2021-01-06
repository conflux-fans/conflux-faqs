<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [如何获取节点程序?](#%E5%A6%82%E4%BD%95%E8%8E%B7%E5%8F%96%E8%8A%82%E7%82%B9%E7%A8%8B%E5%BA%8F)
- [archive node, fullnode, lightnode 的区别 ？](#archive-node-fullnode-lightnode-%E7%9A%84%E5%8C%BA%E5%88%AB-)
- [主网跟测试网的 chain_id 分别是多少](#%E4%B8%BB%E7%BD%91%E8%B7%9F%E6%B5%8B%E8%AF%95%E7%BD%91%E7%9A%84-chain_id-%E5%88%86%E5%88%AB%E6%98%AF%E5%A4%9A%E5%B0%91)
- [主网，测试网的配置文件从哪获取 ?](#%E4%B8%BB%E7%BD%91%E6%B5%8B%E8%AF%95%E7%BD%91%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E4%BB%8E%E5%93%AA%E8%8E%B7%E5%8F%96-)
- [如何运行节点？](#%E5%A6%82%E4%BD%95%E8%BF%90%E8%A1%8C%E8%8A%82%E7%82%B9)
- [如何进行 GPU 挖矿?](#%E5%A6%82%E4%BD%95%E8%BF%9B%E8%A1%8C-gpu-%E6%8C%96%E7%9F%BF)
- [运行节点的机器配置有什么要求?](#%E8%BF%90%E8%A1%8C%E8%8A%82%E7%82%B9%E7%9A%84%E6%9C%BA%E5%99%A8%E9%85%8D%E7%BD%AE%E6%9C%89%E4%BB%80%E4%B9%88%E8%A6%81%E6%B1%82)
- [为什么我运行的是 archive node 但查询较早的 tx 返回 null ？](#%E4%B8%BA%E4%BB%80%E4%B9%88%E6%88%91%E8%BF%90%E8%A1%8C%E7%9A%84%E6%98%AF-archive-node-%E4%BD%86%E6%9F%A5%E8%AF%A2%E8%BE%83%E6%97%A9%E7%9A%84-tx-%E8%BF%94%E5%9B%9E-null-)
- [为什么我的节点经常同步会卡主 ？](#%E4%B8%BA%E4%BB%80%E4%B9%88%E6%88%91%E7%9A%84%E8%8A%82%E7%82%B9%E7%BB%8F%E5%B8%B8%E5%90%8C%E6%AD%A5%E4%BC%9A%E5%8D%A1%E4%B8%BB-)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->




### 如何获取节点程序?
节点程序有多种获取方式，可以[自行编译](https://developer.conflux-chain.org/docs/conflux-doc/docs/installation)，或直接[下载二进制程序](https://github.com/Conflux-Chain/conflux-rust/releases)，或者使用 [docker](https://hub.docker.com/repository/docker/confluxchain/conflux-rust)


### archive node, fullnode, lightnode 的区别 ？
具体参看[这里](https://juejin.cn/post/6854573217655291917)


### 主网跟测试网的 chain_id 分别是多少
主网是 1029(上线日期), 测试网是 1


### 主网，测试网的配置文件从哪获取 ?
通过 github release 下载的 zip 包中有对应的配置文件，主网的是 `tethys.toml` 测试网是 `testnet.toml`


### 如何运行节点？
参看[官方文档](https://developer.conflux-chain.org/docs/conflux-doc/docs/get_started)


### 如何进行 GPU 挖矿?
参看[这篇文章](https://juejin.cn/post/6888565061200773127)


### 运行节点的机器配置有什么要求?
4核处理器，16G内存，200G硬盘


### 为什么我运行的是 archive node 但查询较早的 tx 返回 null ？
你需要打开 `persist_tx_index = true` 配置来存储tx索引以供查询，更改该配置后需要删除所有数据重启节点，否则会卡住


### 为什么我的节点经常同步会卡主 ？
大概率是因为网络问题，可以重启节点