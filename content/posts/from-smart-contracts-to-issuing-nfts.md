---
title: "从智能合约到发行 NFT"
date: 2023-03-05T15:09:00+08:00
tags: ["WEB3.0", "NFT"]
draft: false
toc: true
---

## 引言

这两年 NFT 非常火，但是我一直没有系统的去了解过，今年准备好好学习 WEB3.0 相关的知识，所以就从智能合约和 NFT 入手了。

这篇文章主要是记录我学习智能合约和 NFT 的过程，分享一些学习资料，希望能帮助到大家。

## 智能合约

### 起源

- 智能合约的概念最早是由美国计算机科学家 Nick Szabo 在 1994 年提出的，当时他称其为「合约代码」。
- 在比特币的诞生和普及后，智能合约逐渐得到了广泛的关注和应用。
- 智能合约在区块链技术的基础上实现了不需要第三方的自动执行合同的能力，这是一项非常具有革命意义的技术。

<!--more-->

### 特点

- 去中心化：智能合约的执行不需要任何中间机构或个人的介入。
- 自动执行：智能合约代码在执行过程中不需要人为干预。
- 透明：智能合约的代码和执行结果对所有人都是可见的。
- 安全：智能合约代码是不可更改的，执行结果也是不可篡改的，这保证了合约的安全性。


### 工作原理

智能合约基于区块链技术和分布式计算技术实现。

智能合约被编写为一段计算机程序代码，并被部署在区块链上的智能合约平台中。当用户想要执行一个智能合约时，他们会向合约发送一个交易请求，智能合约会对交易请求进行验证和执行相应的程序代码，并将结果写入区块链上的交易记录中。

智能合约的执行结果会被广播到整个区块链网络中，所有节点进行验证和确认，最终被写入区块链上的区块中。

智能合约具有去中心化、不可篡改、自我执行、高度安全等特点。


### 与传统合约区别

![](https://img.forecho.com/N5YXLz.jpg "智能合约与传统合约区别")

| 特点 |	传统合约	| 智能合约 | 
| ----- | ----- | ----- |
| 执行方式	| 人工执行 |	自动执行 |
| 中心化程度 |	中心化	| 去中心化 |
| 可信度 |	风险较高	| 高度可信 |
| 成本和效率 |	成本高、效率低 |	成本低、效率高 |
| 应用范围	| 法律和商业领域 |	广泛应用 |


### 应用

智能合约在什么地方有应用

- **区块链金融**：用于数字货币、智能投资、分布式交易、借贷、保险等金融领域，以提高金融交易的效率和安全性。
- **物联网**：用于智能家居、智能城市、智能工厂等物联网场景，以实现设备之间的自动化交互和数据共享。
- **版权保护**：用于音乐、电影、游戏等知识产权保护领域，以确保创作者能够获得他们应得的收益。
- **医疗卫生**：用于电子病历、药品追溯、医疗数据管理等领域，以提高医疗卫生服务的质量和安全性。
- **政务服务**：用于政务服务领域，例如政府采购、社会保障、土地管理等领域，以提高公共服务的效率和透明度。
- **游戏业**：用于游戏领域，例如游戏道具交易、游戏账号管理等领域，以提高游戏交易的安全性和透明度。

## Ethereum 与智能合约的关系

Ethereum（ETH）是一种基于区块链技术的数字货币，同时也是一种开源的区块链平台，其中最重要的功能之一就是支持智能合约的开发和运行。因此，ETH 和智能合约有着密切的关系。

ETH 通过引入智能合约技术，使得开发者可以在区块链上编写和运行自己的应用程序，这些应用程序被称为「去中心化应用（DApps）」。智能合约是这些 DApps 的核心组件，它们是一种能够自动执行合约条款的计算机程序，其执行过程基于区块链上的共识机制，并且可以实现可信的去中心化的交易和信息交互。

因此，ETH 和智能合约的结合，使得 ETH 平台上可以实现各种去中心化的应用，例如数字货币交易、投资管理、分布式市场、金融衍生品、电子投票、数字身份认证等，这些应用都依赖于智能合约来实现自动化的执行和管理。所以说，智能合约是 ETH 平台的重要组成部分，也是 ETH 成功的重要原因之一。

## 如何编写智能合约

### 编程语言

智能合约的编程语言有很多种，但是目前最常用的是 Solidity。其他还有：

- Vyper
- Chaincode

### 开发环境

- 本地搭建（不推荐）
- 在线 IDE
	- [Remix](https://remix.ethereum.org/)：适合简单的合约开发
		- 无法长期存储，容量 5M
		- 插件多，可以直接部署调试
	- [Replit](https://replit.com/)：适合较复杂的合约开发
		- 支持线上 Solidity 开发部署
		- 支持 UI 可视化交互
- 框架开发
	- [Hardhat](https://hardhat.org/): JavaScripe
	- [Brownie](https://eth-brownie.readthedocs.io/): Python

### Solidity

Solidity 是一种面向对象的、基于合约的、高级的、静态类型的、通用的、编译型的、类 C 风格的语言，它是一种用于编写智能合约的语言。

这门编程语言总共包含四种不同的重要元素，Contract、Variable、Function 和 Event。

### Contract Examples

```solidity
// SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.8.2 <0.9.0;

/**
 * @title Storage
 * @dev Store & retrieve value in a variable
 * @custom:dev-run-script ./scripts/deploy_with_ethers.ts
 */
contract Storage {

    uint256 number;

    /**
     * @dev Store value in variable
     * @param num value to store
     */
    function store(uint256 num) public {
        number = num;
    }

    /**
     * @dev Return value 
     * @return value of 'number'
     */
    function retrieve() public view returns (uint256){
        return number;
    }
}
```

开头写明合约协议，接着要写 solidity 版本号，这个合约的功能是存储和读取一个数字，这个数字是一个 uint256 类型的，也就是 256 位的无符号整数，这个数字可以被任何人存储和读取。

### ABI

ABI（Application Binary Interface 是指在两个不同程序之间进行交互的规范，其定义了函数调用的参数、返回值、异常处理等方面的约定。

在以太坊中，ABI 是指用于定义智能合约与外部世界进行交互的规范。智能合约在部署时会生成一个 ABI，其他应用程序可以通过 ABI 来与智能合约进行交互。

ABI 定义了智能合约的接口，包括智能合约的函数名、参数类型和返回值类型等信息，通过 ABI，外部应用程序可以调用智能合约中的函数，并获得相应的返回值。

通常，编写智能合约时会生成一个 ABI 文件，我们可以将这个文件提供给客户端库，以便客户端库生成相应的接口，来与智能合约进行交互。

### 发布

合约一般先发布到测试网络，测试通过后再发布到主网。发布之前我们需要准备：

- 钱包，测试网钱包：可以使用 MetaMask 创建帐号
- 水龙头，测试网的 Gas fee：可以去[官网](https://ethereum.org/en/developers/docs/networks/#goerli)领取

![](https://img.forecho.com/w3CqZW.png)

步骤：

- 先在 Remix 中编写合约
- 点击 Compile，编译合约
- 点击 Deploy，部署合约

任何操作都可以看日志输出的信息，发布成功之后可以去 Etherscan 查看合约信息。可以查看[我发布的合约](https://goerli.etherscan.io/tx/0xea4624df27ef42d1cb1f410fe4885a01d1aa88c9619628895e87c8b88d0c699d)。

## OpenZeppelin

![](https://img.forecho.com/8u47Vy.jpg)


[OpenZeppelin](https://www.openzeppelin.com/contracts) 是一个开源的以太坊智能合约开发库，提供安全、可靠和易于使用的智能合约组件和工具，帮助开发者构建更安全的智能合约。它提供了许多常用的智能合约组件，例如 ERC20、ERC721、SafeMath、AccessControl 等，可以帮助开发者减少重复造轮子的时间和成本，同时提高智能合约的安全性。

## NFT 合约

![](https://img.forecho.com/Ad65Cv.png)

通过 OpenZeppelin 生成我们 NFT 的初始化代码，代码复制到 Remix

然后修改代码，通过编译检查代码，然后发布。

![](https://img.forecho.com/Az8CyB.jpg)

测试合约：

- 点击 safeMint 右边展开，approve 输入我们要 Mint 的钱包地址，比方说 `0x7f303C3cA6FE9C2A3ebabDeC69c54FE8102bDb9F` ；uri 输入 NFT 的 metadata URL（可选操作，不然 NFT 没有图片）；然后点击 transact 开始 mint；
- 检查日志，确认 Mint 状态
- 去 [OpenSea](https://testnets.opensea.io/zh-CN) 输入钱包地址，Check NFT。

![](https://img.forecho.com/iZId5n.jpg)


[这个](https://testnets.opensea.io/zh-CN/assets/goerli/0xc54d3a726014f1cf33eb87621fe3a9839d180e3f/0)就是我发布的 NFT。

当然我上面为了演示效果，搞清楚流程。实际的合约代码要比这个复杂的多，最主要的是还要考虑安全性，并且还要做一些限制等等，以后有机会再进一步研究分享。


智能合约都是开源的，附上两个智能合约链接，有兴趣可以研究研究。

- [BDUCK](https://etherscan.io/address/0x71e7afa8b3ab8e83011ce7bbbdcd76ccd7cb0660#code)
- [KIBO](https://polygonscan.com/address/0x65094b37fcc96d6f06384997dae29d4401be0b3e#code)

## 总结

本文主要介绍了智能合约和 NFT 的一些基础知识，并且通过实践演示了如何发布一个 NFT 合约，搞清楚了发布流程。

## 参考

- [How to Develop an NFT Smart Contract (ERC721) with Alchemy](https://docs.alchemy.com/lang-zh/docs/how-to-develop-an-nft-smart-contract-erc721-with-alchemy)