---
title: "使用 HardHat 创建 NFT 智能合约"
date: 2023-06-08T18:30:00+08:00
tags: ["WEB3"]
draft: false
toc: true
---

## 引言

最近在学智能合约，本篇文章主要记录如何使用 [HardHat](https://hardhat.org/) 创建 NFT 智能合约。我们还将学习如何使用 Hardhat 写测试合约和部署智能合约。

## 先决条件

- 略懂 [Node.js](https://nodejs.org/)
- 略懂 [Solidity](https://docs.soliditylang.org/)
- 略懂什么是区块链钱包，比如 [MetaMask](https://metamask.io/)

## 搭建环境

### 安装 Node.js

首先，我们需要安装 [Node.js](https://nodejs.org/)，参照官网的安装教程即可。

### MetaMask

- 安装并下载 [MetaMask](https://metamask.io/) 浏览器插件，然后创建一个钱包。用于与区块链交互。
- 安装完 MetaMask 后，添加与 [Polygon Mumbai 测试网](https://docs.unstoppabledomains.com/manage-domains/guides/add-polygon-to-metamask/)的连接。另外也可以使用 [Chainlist](https://chainlist.org/?testnets=true&search=Mumbai) 快捷添加。
- 使用 [Polygon 水龙头](https://mumbaifaucet.com/)为你的账户充值测试网的 MATIC 代币。

<!--more-->

## 创建 Polygonscan API 密钥

当我们将我们的合约部署到区块链（主网或测试网）时，部署后验证我们的智能合约代码是一种最佳实践。如果我们的智能合约被验证了，那么智能合约代码将在区块浏览器上可见，用户将能够直接从区块浏览器（如 Polygonscan）与智能合约交互。验证源代码是非常被鼓励的，因为它使我们的项目更透明，用户更有可能与之交互。

使用 HardHat 插件，智能合约可以在部署过程中自动进行验证。为此，我们需要一个 Polygonscan API 密钥。按照以下步骤获取你自己的 API 密钥：

- 打开 [Polygonscan](https://polygonscan.com/)。
- 点击页面右上角的 `SignIn`。
- 如果你已经有账号，输入你的用户名和密码进行登录，否则通过访问 [Register](https://polygonscan.com/register) 创建你的新账号。
- 一旦你登录了，转到左侧边栏的 `API Keys` 部分。
- 点击`Add`按钮，给它取个名字，然后点击继续。

现在你有了一个 API 密钥，这将允许你访问 Polygonscan API 的功能，如合约验证。这个密钥对主网和测试网都是一样的。

## 创建一个 HardHat 项目

安装 HardHat，运行命令：

```shell
npm install -g hardhat
```

这将全局安装 HardHat，以便我们后来可以使用 npx 命令来创建 HardHat 项目。

现在，我们将使用以下代码创建我们的项目：

```shell
mkdir art_gallery # 我将我的项目文件夹命名为 art_gallery，但其他任何名称都可以
cd art_gallery    # 进入目录
npx hardhat
```

输入最后一个命令后，类似于以下的内容应该出现在你的屏幕上：

![](https://img.forecho.com/zLo4lQ.png)

这里我选的是 typescript，你可以根据自己的喜好选择。

## 理解代码
现在让我们打开我们的项目并看看它包含什么。我将使用 VSCode 作为我的编辑器，但你可以自由地使用你感觉舒服的任何其他代码编辑器。

![](https://img.forecho.com/gXb25A.png)


我们得到的是一个非常简单的项目脚手架。所有我们的智能合约、脚本文件和测试脚本都将保存在它们各自的目录（文件夹）中。

`hardhat.config.js` 文件包含了所有特定于 HardHat 的配置。

在我们开始编写我们的智能合约之前，让我们看一下 `hardhat.config.js` 文件，这是我们 HardHat 项目的核心。这个文件的默认内容是：

```js
import { HardhatUserConfig } from "hardhat/config";
import "@nomicfoundation/hardhat-toolbox";

const config: HardhatUserConfig = {
  solidity: "0.8.18",
};

export default config;
```

## 安装 OpenZeppelin 库

在编写任何程序时，我们总是倾向于使用各种库，这样我们就不必从头开始编写。由于我们将构建一个基于 NFT 的项目，我们将遵循在 [EIP-721](https://eips.ethereum.org/EIPS/eip-721) 中定义的标准。最好的方式是导入 OpenZeppelin 合约库中的 ERC721 合约，并只对我们的项目进行必要的修改。要安装这个包，打开终端并运行命令：

```shell
npm install @openzeppelin/contracts
```

## 开始我们的智能合约

让我们在 `contracts` 目录中创建一个名为 `Artwork.sol` 的新文件。这将是我们的第一个智能合约，它将帮助我们创建 NFTs。

```solidity
//SPDX-License-Identifier: Unlicense
pragma solidity ^0.8.9;

contract Artwork {}
```
我们首先定义我们的智能合约的许可证。对于这个教程，我们将其保留为未许可。如果我们不定义许可证，它将在编译时引起警告。`pragma` 关键字用于定义用于编译代码的 Solidity 版本。

接下来，我们将从我们刚刚安装的 OpenZeppelin 库中导入 ERC721 智能合约。在定义 Solidity 版本的行之后和定义合约之前，导入 ERC721 合约：

```solidity
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
```
如果使用 VSCode，我们需要在 `.vscode/settings.json` 文件中添加以下配置：

```json
"solidity.remappingsUnix": ["@openzeppelin/=node_modules/@openzeppelin/"]
```

参考 [Source "@openzeppelin/contracts...." not found: File import callback not supported](https://github.com/juanfranblanco/vscode-solidity/issues/320#issuecomment-1290891271)

### 继承 ERC721 和构造器初始化

对代码做出以下修改：

```solidity
//SPDX-License-Identifier: Unlicense
pragma solidity ^0.8.9;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

contract Artwork is ERC721 {

    constructor(
        string memory name,
        string memory symbol
    ) ERC721(name, symbol) {}

}
```
这里我们正在做以下几件事：

- 使用 `is` 关键字将 OpenZeppelin 的 ERC721 智能合约继承到我们的 `Artwork.sol` 智能合约中。
- 构造器总是在部署智能合约时首先被调用的函数。由于我们正在继承另一个智能合约，我们必须在定义我们的构造器时传入那个智能合约的构造器的值。这里我们将一个名称和符号作为构造器参数，然后将它们传递给 ERC721 的构造器。
- `name` 和 `symbol` 分别将是我们 NFT 的名称和符号。

### 定义 tokenCounter

NFT 被称为非同质化代币，因为每一个都是独一无二的。使它们独一无二的是赋予它们的代币 id。我们将定义一个名为 tokenCounter 的全局变量，并用它来计算代币 id。它将从零开始，每创建（或"铸造"）一个新的 NFT，它就增加 1。在构造器中，tokenCounter 的值被设置为 0。

```solidity
//SPDX-License-Identifier: Unlicense
pragma solidity ^0.8.9;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

contract Artwork is ERC721 {

    uint256 public tokenCounter;

    constructor(
        string memory name,
        string memory symbol
    ) ERC721(name, symbol) {
        tokenCounter = 0;
    }

}
```

### 创建 mint 函数

现在我们将定义一个 mint 函数，任何用户都可以调用它来铸造新的 NFT。每个 NFT 都会有一些关联的数据。在我们的情况下，我们使用图像或其他收藏品作为 NFT 的基础，因此图像应该以某种方式存储在智能合约中。由于直接在区块链上存储数据有相关的成本，如果存储整个图像和其他关联数据（元数据），那么在财务上将不可行。所以，我们需要单独托管图像以及包含所有 NFT 详细信息的 JSON 文件。图像和 JSON 文件可以分别使用去中心化（使用 IPFS）或传统方法集中托管。JSON 文件也包含指向图像的链接。一旦托管了 JSON 文件，指向该 JSON 文件的链接就存储在区块链中，作为 tokenURI。URI 代表"通用资源标识符"。[以下](https://kongz.herokuapp.com/api/metadata/1)是集中托管 tokenURI 的一个例子。

有了这个思路，mint 函数就是我们创建与智能合约关联的每个 NFT 的方式：

```solidity
function mint(string memory _tokenURI) public {
    _safeMint(msg.sender, tokenCounter);
    _setTokenURI(tokenCounter, _tokenURI);

    tokenCounter++;
}
```

`_safeMint` 是 OpenZeppelin ERC721 合约中的另一个函数，用于铸造新的 NFT。它需要两个参数：
- `to`：第一个参数是一个账户的地址，该账户将在 NFT 铸造后拥有它。
- `tokenId`：第二个参数是新铸造的 NFT 的 tokenId。

`msg.sender` 是一个特殊的关键字，它返回调用智能合约的账户的地址。在这种情况下，它将返回当前调用 mint 函数的账户。因此，调用 mint 函数的账户将作为第一个参数传递，所以铸造的 NFT 将由这个账户拥有。

`_setTokenURI()` 函数还没有定义，所以暂时忽略它。这个函数将用于设置铸造的 NFT 的 tokenURI。这个函数在 ERC721 库中存在，但在 Solidity 版本 0.8.0 之后已经被废弃，所以我们需要自己实现它。

一旦代币被铸造并设置了其 tokenURI，我们就将 tokenCounter 增加 1，以便下一个铸造的代币有一个新的代币 id。

### 创建 `_setTokenURI()` 函数

我们的 NFT 智能合约必须存储所有有效的 tokenId 及其各自的 tokenURI。为此，我们可以使用 Solidity 中的 `mapping` 数据类型。映射的工作方式类似于 Java 等其他编程语言中的 hashmap。我们可以定义一个从 `uint256` 数到 `string` 的映射，这将表明每个 tokenId 都映射到其各自的 tokenURI。在声明 tokenCounter 变量之后，定义映射：

```solidity
mapping (uint256 => string) private _tokenURIs;
```

现在让我们编写_setTokenURI 函数：

```solidity
function _setTokenURI(uint256 _tokenId, string memory _tokenURI) internal virtual {
    require(
        _exists(_tokenId),
        "ERC721Metadata: URI set of nonexistent token"
    );  // Checks if the tokenId exists
    _tokenURIs[_tokenId] = _tokenURI;
}
```

这里定义了许多新的术语，所以让我们逐一处理：

- `internal`：这个函数用 internal 关键字定义。这意味着这个函数只能由这个智能合约中的其他函数或继承这个智能合约的其他智能合约调用。这个函数不能被外部用户调用。
- `virtual`：这个关键字意味着这个函数可以被任何继承这个智能合约的合约重写。
- `require`：函数体内的第一件事就是 `require` 关键字。它接收一个条件语句。如果这个语句返回 true，那么就执行函数体的其余部分。如果条件语句返回 false，那么它会生成一个错误。第二个参数是生成的错误消息，它是可选的。
- `_exists()`：如果已经铸造了传入的 tokenId 的代币，那么这个函数返回 true，否则返回 false。

总结：这个函数首先确保我们试图设置 tokenURI 的 tokenId 已经被铸造。如果是，它将把 tokenURI 添加到映射中，以及相应的 tokenId。

### 创建 `tokenURI()` 函数

我们需要创建的最后一个函数是 `tokenURI()` 函数。它将是一个公共可调用的函数，接受一个 tokenId 作为参数，并返回其相应的 tokenURI。这是一个标准的函数，被 OpenSea 等基于 NFT 的平台调用。像这样的平台使用这个函数返回的 tokenURI 来显示有关 NFT 的各种信息，如其属性和显示图像。

让我们编写 tokenURI 函数：

```solidity
function tokenURI(uint256 _tokenId) public view virtual override returns(string memory) {
    require(
        _exists(_tokenId),
        "ERC721Metadata: URI set of nonexistent token"
    );
    return _tokenURIs[_tokenId];
}
```

- `public`：这个函数是公开的，这意味着任何外部用户都可以调用它。
- `view`：由于此函数不改变区块链的状态，即它不改变智能合约中的任何值，执行此函数不需要任何 Gas。由于不会发生任何状态更改，因此此函数被定义为 view。
- `override`：我们已经在我们继承的 ERC721 合约中有一个 tokenURI() 函数，它使用「baseURI + tokenId」的概念来返回 tokenURI。由于我们需要不同的逻辑，我们需要使用此关键字覆盖继承的函数。
- `returns(string memory)`：由于此函数将返回一个字符串值，我们在声明函数时必须定义它。`memory` 关键字定义了信息的存储位置。

这个函数首先检查是否铸造了传入的 tokenId。如果已经铸造了代币，它从映射中返回 tokenURI。

### 将所有功能组合在一起

将所有函数组合在一起，最终的智能合约将如下所示：

```solidity
//SPDX-License-Identifier: Unlicense
pragma solidity ^0.8.9;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

contract Artwork is ERC721 {

    uint256 public tokenCounter;
    mapping (uint256 => string) private _tokenURIs;

    constructor(
        string memory name,
        string memory symbol
    ) ERC721(name, symbol) {
        tokenCounter = 0;
    }

    function mint(string memory _tokenURI) public {
        _safeMint(msg.sender, tokenCounter);
        _setTokenURI(tokenCounter, _tokenURI);

        tokenCounter++;
    }

    function _setTokenURI(uint256 _tokenId, string memory _tokenURI) internal virtual {
        require(
            _exists(_tokenId),
            "ERC721Metadata: URI set of nonexistent token"
        );  // Checks if the tokenId exists
        _tokenURIs[_tokenId] = _tokenURI;
    }

    function tokenURI(uint256 _tokenId) public view virtual override returns(string memory) {
        require(
            _exists(_tokenId),
            "ERC721Metadata: URI set of nonexistent token"
        );
        return _tokenURIs[_tokenId];
    }

}
```

## 编译智能合约

现在我们的智能合约已经准备好了，我们必须将其编译。为了使用 HardHat 编译一个智能合约，请运行以下命令：

```bash
npx hardhat compile
```

如果一切顺利，你将会看到「Compilation finished successfully」的信息。如果合约没有成功编译或者出现了错误，你可以尝试再次阅读本教程，找出出错的地方。一些可能出现的错误包括：

- 没有提供 `SPDX-License-Identifier`
- 使用 pragma 关键字定义的 Solidity 编译器版本和在 `hardhat.config.js` 中定义的版本不匹配。
- 导入的智能合约的 Solidity 版本和编写我们的智能合约使用的版本不匹配。为解决这个问题，你需要仔细检查你用 npm 安装的 OpenZeppelin 合约的版本。在我这里，npm 包的版本是 8.19.3'，智能合约是用 solidity 版本 0.8.18 编写的。

## 测试智能合约

到目前为止，我们已经编写了我们的智能合约并编译了它。然而，一个成功编译的智能合约并不意味着它是正确的！编写测试用例以确保它通过所有预期的使用情况和一些边缘情况是非常重要的。由于一旦智能合约被部署到区块链上就不能被修改，因此测试智能合约变得更加重要。

我们将使用 `chai` 库来编写我们的测试。如果在创建项目时没有安装这个库，你可以使用命令 `npm install --save-dev chai` 来安装。

我们将对我们的智能合约进行以下测试：

- NFT 成功地被铸造：在一个账户调用铸造函数后，该账户应该拥有一个新铸造的 NFT 并且其余额会增加。
- tokenURI 被成功地设置：对于使用不同 tokenURIs 铸造的两个 token，两个 token 应该各自拥有自己的 tokenURI，并且可以正确地获取数据。


### 编写测试用例

在 test 目录下创建一个新的文件，叫做 Artwork.ts。文件名并不重要，但为了保持有序，测试文件的名称应该和被测试的合约有所关联。在这个新文件中，添加以下代码：

```typescript
import { expect } from "chai";
import { ethers } from "hardhat";

describe("Artwork Smart Contract Tests", function () {
    this.beforeEach(async function () {
        // This is executed before each test
    })

    it("NFT is minted successfully", async function () {

    })

    it("tokenURI is set sucessfully", async function () {

    })
});
```

- `describe`：此关键字用于给我们将要执行的测试集命名。
- `beforeEach`：在 `beforeEach` 中定义的函数将在每个测试用例之前执行。我们将在这里部署 NFT 合约，因为每次运行测试之前必须部署合约。
- `it`：这个用来编写每个测试用例。`it` 函数接受一个测试的标题和一个运行测试用例的函数。

注意：与 Truffle 不同，HardHat 不需要单独为测试运行 `ganache-cli`。Hardhat 有自己的本地测试网，我们可以使用。

### 部署合约和编写测试

为了部署智能合约，我们首先需要使用 `ethers.getContractFactory()` 获取对编译好的智能合约的引用，然后我们可以使用 `deploy()` 方法来部署智能合约并传入参数。我们在 `beforeEach()` 部分做这个操作。

```javascript
let artwork;

this.beforeEach(async function() {
    // This is executed before each test
    // Deploying the smart contract
    const Artwork = await ethers.getContractFactory("Artwork");
    artwork = await Artwork.deploy("Artwork Contract", "ART");
})
```

为了检查 NFT 是否正确地被铸造，我们首先获取 HardHat 创建的一个默认账户。然后我们调用智能合约中的 mint 函数，传入一个随机的 tokenURI。我们在铸造之前和之后检查账户的余额，它们应分别为 0 和 1。如果合约通过了测试，那就意味着 NFT 被正确地铸造了。

```javascript
it("NFT is minted successfully", async function() {
    [account1] = await ethers.getSigners();

    expect(await artwork.balanceOf(account1.address)).to.equal(0);
    
    const tokenURI = "https://kongz.herokuapp.com/api/metadata/1"
    const tx = await artwork.connect(account1).mint(tokenURI);

    expect(await artwork.balanceOf(account1.address)).to.equal(1);
})
```

为了检查 tokenURI 是否被正确设置，我们取两个随机的 tokenURIs 并从不同的账户设置它们。然后我们调用 `tokenURI()` 函数来获取相应 token 的 tokenURI，然后将它们与传入的参数进行匹配，以确保 tokenURIs 被正确地设置。

```javascript
it("tokenURI is set sucessfully", async function() {
    [account1, account2] = await ethers.getSigners();

    const tokenURI_1 = "https://kongz.herokuapp.com/api/metadata/1"
    const tokenURI_2 = "https://kongz.herokuapp.com/api/metadata/2"

    const tx1 = await artwork.connect(account1).mint(tokenURI_1);
    const tx2 = await artwork.connect(account2).mint(tokenURI_2);

    expect(await artwork.tokenURI(0)).to.equal(tokenURI_1);
    expect(await artwork.tokenURI(1)).to.equal(tokenURI_2);

})
```

### 将所有内容放在一起

最终，在将所有测试用例组合在一起后，Artwork.ts 文件的内容将是：

```typescript
import { expect } from "chai";
import { ethers } from "hardhat";
import { Artwork } from "../typechain-types";

describe("Artwork Smart Contract Tests", function () {
    let artwork: Artwork;

    this.beforeEach(async function () {
        // This is executed before each test
        // Deploying the smart contract
        const Artwork = await ethers.getContractFactory("Artwork");
        artwork = await Artwork.deploy("Artwork Contract", "ART");
    })

    it("NFT is minted successfully", async function () {
        let account1;
        [account1] = await ethers.getSigners();

        expect(await artwork.balanceOf(account1.address)).to.equal(0);

        const tokenURI = "https://kongz.herokuapp.com/api/metadata/1"
        const tx = await artwork.connect(account1).mint(tokenURI);

        expect(await artwork.balanceOf(account1.address)).to.equal(1);
    })

    it("tokenURI is set successfully", async function () {
        let account1;
        let account2;
        [account1, account2] = await ethers.getSigners();

        const tokenURI_1 = "https://kongz.herokuapp.com/api/metadata/1"
        const tokenURI_2 = "https://kongz.herokuapp.com/api/metadata/2"

        const tx1 = await artwork.connect(account1).mint(tokenURI_1);
        const tx2 = await artwork.connect(account2).mint(tokenURI_2);

        expect(await artwork.tokenURI(0)).to.equal(tokenURI_1);
        expect(await artwork.tokenURI(1)).to.equal(tokenURI_2);

    })
});
```

您可以使用命令运行测试：

```bash
npx hardhat test
```


## 部署智能合约

到目前为止，我们已经学会了如何编写智能合约并对它们进行测试。现在我们终于可以开始将我们的智能合约部署到 Mumbai 测试网络了，这样我们就可以向我们的朋友们炫耀我们新学到的技能了😎。

在我们继续并部署我们的智能合约之前，我们将需要两个额外的 npm 包：

- `dotenv`：输入 `npm install dotenv`。这将用于管理环境变量，这些变量用于设置我们的 Polygonscan API 密钥的访问。
- `@nomiclabs/hardhat-etherscan`：输入 `npm install @nomiclabs/hardhat-etherscan`。这个库用于在将智能合约部署到网络时验证它。在 Etherscan 和 Polygonscan 上验证智能合约的过程是一样的。

### 设置环境变量

在项目根目录中创建一个名为 `.env` 的新文件。

在 `.env` 文件中创建一个名为 POLYGONSCAN_KEY 的环境变量，将其设置为教程开始时创建的 API 密钥。同时添加另一个条目 PRIVATE_KEY，将其设置为有 MATIC 的 Mumbai 测试网钱包帐户的私钥。
```
POLYGONSCAN_KEY=xxxx
PRIVATE_KEY=xxxxx
```

### 修改配置文件

为了将经过验证的智能合约部署到 Mumbai 测试网络，我们必须在 hardhat.config.js 文件中做一些改动。首先，将这段完整的代码复制粘贴到文件中，然后我们会一步一步地解释这段代码，以理解正在发生的事情：

```typescript
import { HardhatUserConfig } from "hardhat/config";
import "@nomicfoundation/hardhat-toolbox";
require("dotenv").config();

const config: HardhatUserConfig = {
  solidity: "0.8.18",
  networks: {
    mumbai: {
      url: "https://matic-testnet-archive-rpc.bwarelabs.com",
      accounts: [
       `${process.env.PRIVATE_KEY}`
      ]
    }
  },
  etherscan: {
    apiKey: process.env.POLYGONSCAN_KEY,
  }
};

export default config;
```


由于我们将把我们的合约部署到 Mumbai 测试网，所以我们在网络部分创建了一个新的网络对象。我们将其命名为 mumbai，并将其 url 设置为 Mumbai 测试网的 RPC url。然后，我们将我们的私钥添加到 accounts 数组中，以便我们可以使用它来部署我们的智能合约。

最后，我们将我们的 API 密钥添加到 etherscan 对象中，以便我们可以在部署智能合约时验证它。

### 编写部署脚本

在 `scripts` 文件夹中创建一个名为 `deploy_artwork.ts` 的新文件。在这个文件中，我们将编写一个脚本，用于部署我们的智能合约。

```typescript
import { ethers, run } from "hardhat";

async function main() {
  const ContractFactory = await ethers.getContractFactory("Artwork");
  const contract = await ContractFactory.deploy("Artwork Contract", "ART");


  // Wait for the contract to be mined and get the contract's deployed bytecode
  await contract.deployed();

  console.log("Contract deployed to:", contract.address);

  // wait 1 minute for the contract to be mined
  await new Promise((r) => setTimeout(r, 60000));

  // Verify the contract
  try {
    await run("verify:verify", {
      address: contract.address,
      constructorArguments: ["Artwork Contract", "ART"],
    });
    console.log(`Contract verified successfully.`);
  } catch (error) {
    console.error("Failed to verify contract:", error);
  }
}

main()
    .then(() => process.exit(0))
    .catch((error) => {
      console.error(error);
      process.exit(1);
    });

```

执行部署脚本：

```bash
npx hardhat run scripts/deploy_artwork.ts --network mumbai
```

解释一下上面的代码：

- 我们首先使用 `getContractFactory` 方法获取我们的智能合约的工厂对象。
- 然后，我们使用 `deploy` 方法部署我们的智能合约。我们将合约的名称和符号作为参数传递给构造函数。
- 最后，我们使用 `verify:verify` 任务验证我们的智能合约。我们将合约的地址和构造函数的参数作为参数传递给任务。

需要说明的是：

- `deploy` 之后要等 1 分钟，因为部署智能合约需要一些时间。如果我们立即验证智能合约，我们将得到一个错误，因为智能合约还没有被部署。
- 当然验证合约你也可以通过命令完成，不写代码，验证的命令如下：

```bash
npx hardhat verify --network sepolia <contract address> "Artwork Contract" "ART"
```

## 与智能合约互动

如果我们在 Polygonscan 上查看我们的智能合约，你可以看到我们的合约已经通过验证。这在 Contracts 标签页中有一个绿色的对号表示。在 Contracts 标签页中，点击 Write Contract。现在点击「Connect to Web3」并连接你的 
Metamask 账户。

我刚才部署的智能合约的地址是：[0x7d64B6EDAcE2Cf8FfE57199406861E9fcEeb7364](https://mumbai.polygonscan.com/address/0x7d64B6EDAcE2Cf8FfE57199406861E9fcEeb7364)

![](https://img.forecho.com/8HcEux.png)

现在选择 mint 操作并输入你的 tokenURI。我使用 https://kongz.herokuapp.com/api/metadata/5 作为我的 tokenURI。如果你愿意，你可以通过使用 IPFS 来托管你自己的 tokenURI。一旦你输入你的 tokenURI，点击「Write」按钮。这将显示一个 Metamask 弹出窗口。确认交易并等待其被确认。

恭喜你🥳🥳🥳，你的 NFT 已经成功铸造。你可以访问 [Opensea Testnet](https://testnets.opensea.io/zh-CN) 页面，用钱包登录之后，在「Profile」部分现在你可以查看你的 NFT。可以在这里查看示例。

![](https://img.forecho.com/KV0LUp.png)

- [CyberKong #7 地址](https://testnets.opensea.io/assets/mumbai/0x7d64b6edace2cf8ffe57199406861e9fceeb7364/2)
- [CyberKong #2 地址](https://testnets.opensea.io/assets/mumbai/0x7d64b6edace2cf8ffe57199406861e9fceeb7364/0)

## 结论

在本教程中，我们学习了 HardHat 的一些基础知识。我们编写了一个可以用于创建 NFT 的智能合约，为我们的智能合约编写了测试，最后将其部署到 Mumbai 测试网。我们还使用了 HardHat 插件和 Polygonscan API 密钥验证了我们的合约。使用类似的程序，我们可以构建任意数量的 DeFi 项目并部署到任何兼容 EVM 的网络（Ethereum, Polygon, Binance Smart Chain, Avalanche 等）。

## 参考资料

在学习过程中，我发现以下资料非常有用：

- [Hardhat 官方文档](https://hardhat.org/)
- [Polygon Wiki](https://wiki.polygon.technology/docs/home/new-to-polygon/)

## 最后

本篇文章大部分内容都翻译自 [《Create an NFT smart contract with HardHat》](https://learn.figment.io/tutorials/create-nft-smart-contract-with-hardhat)，并且结合我自己的实践做了一些修改，希望对你有所帮助。

本篇教程的代码已经上传到 Github，地址 [hi-hardhat](https://github.com/forecho/hi-hardhat)。
