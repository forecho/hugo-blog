---
title: "用 PHP 了解区块链基本原型"
date: 2018-02-03T15:29:00+08:00
tags: ["区块链"] 
draft: false
toc: true
---

## 引言

最近这一个月一有时间就研究区块链的技术，看了不少文章，理论知识了解了不少。今天终于有时间写写『如何用 PHP 来了解区块链的基本原理』了。

本篇文章适合对区块链感兴趣而且还需要对区块链已经有一定了解的小伙伴。废话不多说，我们直接上代码。

## Block 区块

区块链由N个区块组成，在区块链里，价值信息存储在区块之中。比如，比特币的区块存储交易记录，而交易记录是任何加密货币的核心。除此之外，区块里还包含有技术信息，比如它的版本号，当前的时间戳，以及上一个区块的哈希（Hash）。

<!--more-->

在这篇文章中，我们所实现的并不是像比特币那样完整的区块链，而是一个简化版本的区块链，它只含有最基本的核心信息。差不多是这样：

```php
class Block
{
    /**
     * @var string 时间
     */
    public $timestamp;

    /**
     * @var integer 索引
     */
    public $index;

    /**
     * @var string 数据
     */
    public $data;

    /**
     * @var string 上一个哈希值
     */
    public $prevHash;

    /**
     * @var string 当前哈希
     */
    public $hash;

    public function __construct($index, $timestamp, $data, $prevHash = '')
    {
        $this->index = $index;
        $this->timestamp = $timestamp;
        $this->data = $data;
        $this->prevHash = $prevHash;
        $this->hash = $this->calculateHash();
    }
}
```

- `timestamp`是当前的时间戳（即，区块被创建的时间），
- `data` 是区块中包含的价值信息，
- `prevHash` 存储的是上一个区块的哈希
- `hash` 保存的是当前区块的哈希。

在比特币的标配中，timestamp、prevHash、hash 是区块的头部数据（Block headers），构成一个单独的数据结构；而交易记录（Transactions，在我们这个版本中就是 data），是另外一个单独的数据结构。而我们在这里为了简化，把数据结构混在了一起。


那我们如何计算哈希呢？计算哈希的方式是区块链的重要特征之一，也正是这个特性使得区块链如此安全。关键在于，计算哈希是一个计算起来很困难的工作，它需要时间，哪怕是在很快的计算机上（这就是为什么人们要买比 CPU 计算能力更强悍 GPU 甚至专门的 ASIC 芯片做矿机的 原因）。这是故意如此设计的，这么做的结果是，往区块链（数据库）里添加新的区块（数据）有一定的困难，以此保证一旦新的数据被加入，往后很难篡改。以后的文章里会进一步讨论并实现这个机制。

现在呢，我们只需要罢区块里的各个字段关联起来，并在此基础上计算出一个 SHA-256 哈希。让我们调用一下  calculateHash 这个方法：

```php
/**
* 加密算法
* @return string
*/
public function calculateHash()
{
    return hash('sha256', $this->index . $this->prevHash . $this->timestamp . json_encode($this->data));
}
```

有了构造函数，我们可以很简单的创建一个区块：

```php
new Block(1, '2017-02-23', ['amount' => 1])
```

## Blockchain 区块链

区块链的本质是保存着特定的数据结构的数据库。它是一个有序的，尾部相连的链表。这就意味着区块是以插入的顺序被存储的，每个区块连接着前一个区块。这种结构就可以很快的获取链上的最后一个区块，而且可以很高效地通过 hash 获取区块。

在 PHP 语言中，这种结构可以通过使用 array 来实现：

```php
class BlockChain
{
    /**
     * @var Block[]
     */
    public $chain = [];
}
```

现在我们来写一个添加区块链的方法：

```php
/**
* 添加区块
* @param Block $newBlock
*/
public function addBlock(Block $newBlock)
{
    $newBlock->prevHash = $this->getLatestBlock()->hash;
    $newBlock->hash = $newBlock->calculateHash();
    array_push($this->chain, $newBlock);
}
```

是不是非常简单？要添加一个新的区块，我们就需要一个已存在区块，然而我们的区块链上还没有一个区块！所以，在任何一个区块链中，必须有至少一个区块，这个区块，要是链上的第一个区块，他被称为创始区块(genesis block)。现在我们用构造方法去创建这样一个区块：


```php
class BlockChain
{
    /**
     * @var Block[]
     */
    public $chain = [];

    public function __construct()
    {
        $this->chain = [$this->createGenesisBlock()];
    }

    /**
        * 创世区块
        * @return Block
        */
    public function createGenesisBlock()
    {
        return new Block(0, '2017-01-23', 'forecho', '0');
    }
}
```

为了确认一下区块链是否正常工作，我们写一个方法来验证：

```php
/**
* 验证区块链
* @return bool
*/
public function isChainValid()
{
    for ($i = 1; $i < count($this->chain); $i++) {
        $currentBlock = $this->chain[$i];
        $prevBlock = $this->chain[$i - 1];
        if ($currentBlock->hash !== $currentBlock->calculateHash()) {
            return false;
        }
        if ($currentBlock->prevHash !== $prevBlock->hash) {
            return false;
        }
    }
    return true;
}
```

开始测试：


```php
$blockChain = new BlockChain();
$blockChain->addBlock(new Block(1, '2017-02-23', ['amount' => 1]));
$blockChain->addBlock(new Block(2, '2017-03-23', ['amount' => 3]));
$blockChain->addBlock(new Block(3, '2017-04-23', ['amount' => 20]));

print_r($blockChain);
echo "区块链验证通过吗？" . ($blockChain->isChainValid() ? '通过' : '失败') . PHP_EOL;
$blockChain->chain[1]->data = ['amount' => 2];
$blockChain->chain[1]->hash = $blockChain->chain[1]->calculateHash();
echo "区块链验证通过吗？" . ($blockChain->isChainValid() ? '通过' : '失败') . PHP_EOL;
```

完整代码去[forecho/hi-Blockchain](https://github.com/forecho/hi-Blockchain) 看，运行结果看[这里](https://ideone.com/CDGIju)。

以上就是区块链的基本原理了，是不是很简单，但实际上真实的区块链远比这个要复杂。在我们的区块链里，添加一个新区块非常快，非常容易；但是在真正的区块链中添加一个新的区块需要更多的工作：在获得添加区块的允许之前要做很繁重的计算才行（这个过程被称为工『作证明机制』，即，『Proof-of-Work』，POW）。并且，区块链是一个没有主权的分布式的数据库。因此，任何一个新的区块在被加入之前，必须经过网络中其它参与者的确认与允许（这个机制被称为『共识机制』，『Consensus』）…… 还有，我们的区块链里，还没有任何交易记录呢！


## 总结

网上看到的最多的是用 Go 来现实，但是我还没来得及去学 Go 语言，无意中看到一个用 JavaScript 实现的版本，好在有一些 JS 基础，看懂了之后觉得『这个 PHP 也能实现嘛』，然后就有了这篇文章。

个人觉得区块链如此之火是因为它是跨界的产物，不仅要学习技术而且还要学习金融。『一头扎进技术里是没有未来的，你技术再好永远有比你好的技术，技术再结合金融领域的知识，就很有意思』这句话是一个老板对我说的，非常赞同。

## 参考连接

- [使用 Go 语言打造区块链（一）](http://lixiaolai.com/2017/09/28/building-blockchain-in-go-part-1/)
- [用 golang 实现区块链系列一 - 基本原型](https://annatarhe.github.io/2017/12/29/building-blockchain-in-go-part-1-basic-prototype.html)
- [Building Blockchain in Go. Part 1: Basic Prototype](https://jeiwan.cc/posts/building-blockchain-in-go-part-1/)
- [[YouTube]Creating a blockchain with Javascript (Blockchain, part 1)](https://www.youtube.com/watch?v=zVqczFZr124&t=10s&index=2&list=PLQ5fjAsCtfQ77yDXk8BVW6sRQO9qzq-Os)