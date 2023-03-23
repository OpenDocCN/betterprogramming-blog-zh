# 设计供应链智能合约

> 原文：<https://betterprogramming.pub/supply-chain-smart-contract-design-e0ae5071bcbe>

## 简化农业供应链网络的运作

![](img/12189ab6578116cf7b54dfc11d77b064.png)

供应链|作者图片

农业在为全球 70 亿公民提供衣食方面发挥着至关重要的作用。超过 25%的世界人口受雇于农业部门。为了满足全球化世界的需求，农业供应链已经形成了漫长而复杂的生产、加工、分销和营销渠道网络。他们由农民、加工商、贸易商、物流供应商、金融公司、消费者和许多其他人组成，每一个人的利益经常是多种多样和相互冲突的。

农业是全球经济的主要贡献者，每年的收入超过 6 万亿美元。这个行业为世界提供了基本商品，如大米、玉米、小麦和牲畜。然而，尽管农业供应链在为全球提供基本产品方面发挥着至关重要的作用，但它面临着许多跨地域和跨商品的共同挑战。

区块链技术创造了单一的真理来源。这对于供应链来说很重要，因为供应链中的多个参与者并不一定相互信任。

在本文中，我们采用了一种实用的方法来研究代码。本文中的例子旨在简化农业供应链的运作。它提高了农民、经销商、零售商和消费者之间的透明度和效率。

# 流程图

![](img/60fb1913d0cc2bff64c6c7feeb5b03b5.png)

流程图

## 功能

1.  Farmer 创建了一个产品，并列出要由经销商购买的产品
2.  农民运送产品
3.  经销商接收产品，加工产品，包装产品，并将其投放市场
4.  零售商从分销商处购买产品
5.  分销商将产品运送给零售商
6.  零售商收到产品并将其出售
7.  消费者购买产品

## 环境设置

让我们克隆存储库并安装依赖项:

```
git clone [https://github.com/ac12644/Supply-Chain.git](https://github.com/ac12644/Supply-Chain.git)
cd Supply-Chain
yarn or npm install
```

现在导航到`/contracts`文件夹，并按给定方式创建文件夹:

您将看到四个文件夹:

*   `/access`文件夹包含农民、经销商、零售商&消费者的角色
*   `/ownership`文件夹包含一个 solidity 文件`Ownable.sol`，将功能限制在选定的地址，类似于你在 [openzeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable.sol) 上看到的
*   `/supplyChain`文件夹包含了链的结构
*   `/utils`文件夹包含一个`Context.sol`，它提供关于当前执行上下文的信息，包括事务的发送者及其数据，取自 [openzeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/Context.sol) 。

首先为角色设计一个智能契约，为此我们使用`/access`文件夹。

## [角色. sol](https://github.com/ac12644/Supply-Chain-Smart-Contract/blob/main/contracts/access/Roles.sol)

该合同将用于管理平台上的角色。它提供了添加、删除和查询角色信息的功能。这将被导入到我们的其他角色智能合同中。

1.  让我们从创建`library Roles`开始:

```
library Roles {
  ...
}
```

2.现在，我们创建`struct Role`来设置地址。为此，您必须创建一个映射，将布尔值中的地址映射到载体。

您必须创建一个映射，将地址映射到载体的布尔值。

```
library Roles {
  struct Role {
    mapping(address => bool) bearer;
  }
  ...
}
```

3.创建具有存储角色的内部函数`add`和账户地址，以检查该地址正在传递未连接的账户且之前未添加:

```
function add( Role storage role, address account) internal {
  require(account != address(0));  
  require(!has(role, account));
}
```

4.类似地，我们创建`remove`函数，在这里我们检查传递的地址不是关联的帐户，并且该地址确实存在于给定的角色中，最后将存储中的角色值设置为 false。

```
function remove( Role storage role, address account) internal {
  require(account != address(0));  
  require(has(role, account));role.bearer[account] = false;
}
```

5.在最后一步，创建一个函数`has`来检查帐户是否有角色，它返回一个布尔值:

```
function remove( Role storage role, address account) 
  internal 
  view 
  returns (bool){
  require(account != address(0));  
  return role.bearer[account];
}
```

6.完成后，您的智能合约看起来如下所示:

角色. sol

## [农民角色](https://github.com/ac12644/Supply-Chain-Smart-Contract/blob/main/contracts/access/FarmerRole.sol)

现在，我们使用之前的`Roles`库合同为农民创建新角色。

1.  让我们从创建一个合同`FarmerRole`文件开始，并使用`struct Role` `Role`库。

```
contract FarmerRole is Context {
  using Roles for Roles.Role
  ...
}
```

2.如果成功添加或删除农民，创建两个事件来捕获。

```
event FarmerAdded(address indexed account); 
event FarmerRemoved(address indexed account);
```

3.通过从`Roles`库`struct Role`继承来定义一个结构`farmers`。

```
Roles.Role private farmers;
```

4.合同的部署地址将被添加为第一个农民。使用协定的构造函数时，必须执行协定的初始设置。使用构造函数将确保在协定创建期间进行初始化。

```
constructor() public { _addFarmer(_msgSender()); }
```

5.定义一个修饰符来检查调用函数的地址是否有合适的角色。

```
modifier onlyFarmer() { 
  require(isFarmer(_msgSender())); 
  _; 
}
```

6.创建一个函数来检查地址是否是 farmer，它返回一个布尔值。

```
function isFarmer(address account) 
  public 
  view 
  returns (bool) 
  { 
    return farmers.has(account); 
  }
```

7.现在，让我们来看看添加和删除`Farmer`角色的函数。

`Farmer`角色智能合约完成，请看下图:

法默罗尔.索尔

类似地，我们创建其他角色`[DistributorRole.sol](https://github.com/ac12644/Supply-Chain-Smart-Contract/blob/main/contracts/access/DistributorRole.sol)`、`[RetailerRole.sol](https://github.com/ac12644/Supply-Chain-Smart-Contract/blob/main/contracts/access/RetailerRole.sol)`和`[ConsumerRole.sol](https://github.com/ac12644/Supply-Chain-Smart-Contract/blob/main/contracts/access/ConsumerRole.sol)`，我们重复`[FarmerRole.sol](https://github.com/ac12644/Supply-Chain-Smart-Contract/blob/main/contracts/access/FarmerRole.sol)`中给出的相同过程，只是根据角色改变函数和变量的名称。

现在，让我们转到主合同`[SupplyChain.sol](https://github.com/ac12644/Supply-Chain-Smart-Contract/blob/main/contracts/supplyChain/SupplyChain.sol)`。

## [供应链](https://github.com/ac12644/Supply-Chain-Smart-Contract/blob/main/contracts/supplyChain/SupplyChain.sol)

这个智能合同包含了供应链背后的所有逻辑。一个农民创造了一种产品并把它放在市场上销售，一个经销商购买它，卖给一个零售商，然后零售商再把它卖给消费者。整个过程是透明的，区块链上的每个人都可以看到，因此没有出错或欺诈的机会。

1.  让我们首先导入我们之前创建的所有必要的契约，并创建一个契约`SupplyChain`。

供应链合同模式

2.存储当前连接的电子钱包的地址。请在之后保存钱包地址。

```
address owner;
```

3.由农民生成的产品代码印在产品上，由经销商验证。`productCode`存储该值。

```
uint256 productCode;
```

4.记录由`Farmer`创建的库存单位数量。`stockUnit`存储该值。

```
uint256 stockUnit;
```

5.创建映射`items`，将产品代码映射到稍后将在`struct`中创建的项目。

```
mapping(uint256 => Item) items;
```

6.定义一个公共映射`itemsHistory`，将产品代码映射到一个数组`TxHash`，该数组跟踪产品在供应链中的行程——从应用程序发送。

```
mapping(uint256 => Txblocks) itemsHistory;
```

7.现在创建一个州，用以下值跟踪产品的生产过程，并将默认州设置为由农民生产。

供应链流程的状态

6.创建一个结构来存储所需的所有产品详细信息。

项目结构

7.创建一个结构来存储`farmer-distributor`、`distributor-retailer`和`retailer-consumer`之间事务的块地址。

```
struct Txblocks { 
  uint256 FTD; // block of farmerToDistributor 
  uint256 DTR; // block of DistributorToRetailer 
  uint256 RTC; // block of RetailerToConsumer 
}
```

8.现在为所有函数创建事件，这些事件将在作业成功完成时发出。

供应链事件

9.创建一个修饰符，检查地址是否是合同的所有者。

```
modifier only_Owner() {
  require(_msgSender() == owner);        
  _;    
}
```

10.定义一个修改量来验证`Caller`，然后定义另一个修改量来检查支付的金额是否足以支付价格。

11.现在，我们定义修改量来检查产品代码是否存在，并更新产品的当前状态。

12.创建一个构造函数来设置所有者、库存单位和产品代码。

```
constructor() public payable {        
  owner = _msgSender();        
  stockUnit = 1;        
  productCode = 1;    
}
```

13.现在，创建一个允许您将地址转换为可支付地址的函数。

```
function _make_payable(address x) internal pure 
returns (address payable) { 
  return payable(address(uint160(x))); 
}
```

14.为所有者创建一个 kill 函数，将合同中存储的所有剩余乙醚发送到所有者的地址。

```
function kill() public { 
  if (_msgSender() == owner) { 
    address payable ownerAddressPayable = _make_payable(owner);
    selfdestruct(ownerAddressPayable); 
  } 
}
```

15.现在，让我们创建一个允许`Farmer`创建产品的函数。

创建产品

16.让我们创建一个函数，允许农民销售您创建的产品，另一个函数允许分销商从农民那里购买产品。

向分销商销售产品的功能

17.当经销商购买产品时，农民需要发货，经销商需要收货。我们可以通过创建发货和收货功能来实现这一点。

18.`Distributor`列出的产品现在必须由`Retailer`购买。在`Retailer`购买产品后，`Distributor`将产品发送给`Retailer`，后者需要接收产品。为此，需要创建三个函数:一个用于`Retailer`购买产品，一个允许`Distributor`运送产品，一个用于`Retailer`接收产品:

加工、包装和销售清单的功能

19.`Distributor`列出的产品现在由`Retailer`购买。在`Retailer`购买后，`Distributor`会运送产品，`Retailer`会收到。为此，需要创建三个函数:一个用于`Retailer`购买产品，一个用于`Distributor`发货，一个用于`Retailer`接收产品:

20.现在，我们还剩两步。第一步是把产品放在零售商的货架上，让它一直放着，直到消费者出现。第二步是让消费者购买该产品。让我们看看如何做到这一点:

21.现在是创建获取产品的函数的时候了:我们创建了三个函数`fetchItemBufferOne`、`fetchItemBufferTwo`和`fetchitemHistory`。

`fetchItemBufferOne`:该功能仅获取创建时生成的产品创建信息。

`fetchItemBufferTwo`:获取产品信息，包括所有参与者的地址

`fetchitemHistory`:获取农民到分销商、分销商到零售商、零售商到消费者的流程的块数据

让我们来看看这些函数:

获取数据的函数

最后，我们的供应链智能合同完成了，你可以在这里看一下最终合同[。](https://github.com/ac12644/Supply-Chain-Smart-Contract/blob/main/contracts/supplyChain/SupplyChain.sol)

# 测试和部署

现在，我们在多边形网络上测试、编译和部署契约。我已经包含了一个[测试文件](https://github.com/ac12644/Supply-Chain-Smart-Contract/blob/main/test/SupplyChain.js)，您可以使用它进行测试。您还必须检查`[truffle-config.js](https://github.com/ac12644/Supply-Chain-Smart-Contract/blob/main/truffle-config.js)`中的配置。

要部署合同，请运行以下命令:

```
truffle develop$truffle(develop)> test
$truffle(develop)> deploy
```

恭喜🤩！您的合同已成功部署。

感谢阅读！

## 想要更多吗？

[](/create-your-initial-coin-offering-ico-contract-in-ethereum-5a94ec3e2337) [## 如何在以太坊创建您的初始硬币发售(ICO)合同

### 让你的众筹更进一步

better 编程. pub](/create-your-initial-coin-offering-ico-contract-in-ethereum-5a94ec3e2337)