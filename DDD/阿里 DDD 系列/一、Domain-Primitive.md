原文链接：[阿里技术专家详解 DDD 系列](https://mp.weixin.qq.com/s?__biz=MzAxNDEwNjk5OQ==&mid=2650403892&idx=1&sn=a91fa477392e80f9420a8ca4d26bcace&chksm=83953c2cb4e2b53a6af3b5a82c3b7d7ed932bfe83f59877a935445ae89edd0ff4ee1c4e82fba&mpshare=1&scene=1&srcid=0603VSMcxv6IQA8gJlOjrPdl&sharer_sharetime=1685777000121&sharer_shareid=fd8ca462f418fde1153c154e83956699#rd)

## 1. DP 是什么？

**Domain Primitive（DP）的意思是 不从任何其他事物发展而来、初级的形成或生长的早期阶段**。

就好像 Integer、String 是所有编程语言的 Primitive 一样，**在 DDD 里， DP 可以说是一切模型、方法、架构的基础**，而就像 Integer、String 一样， DP 又是无所不在的。

Domain Primitive 的定义：**Domain Primitive 是一个在特定领域里，拥有精准定义的、可自我验证的、拥有行为的 Value Object**。

- DP 是一个传统意义上的 Value Object，拥有 Immutable（不可变）的特性；
- DP 是一个完整的概念整体，拥有精准定义；
- DP 使用业务域中的原生语言；
- DP 可以是业务域的最小组成部分、也可以构建复杂组合。

**Domain Primitive 是 Value Object 的进阶版，在原始 VO 的基础上要求每个 DP 拥有概念的整体，而不仅仅是值对象**。在 VO 的 Immutable 基础上增加了 Validity 和行为。当然同样的要求无副作用（side-effect free）。

> Value Object：没有标识符，不可变的对象

## 2. 使用 DP 的三原则

使用 DP 的三原则：

- **让隐性的概念显性化**；
- **让隐性的上下文显性化**；
- **封装多对象行为**。

接下来分别举例解释说明。

### 2.1 让隐性的概念显性化

例子：

- 一个新应用在全国通过 **地推业务员** 做推广，需要做一个用户的 **注册系统**，在用户注册后能够通过用户 **电话号的区号** 对业务员发奖金。

常规写法：

```java
public class User {
    Long userId;
    String name;
    String phone;
    String address;
    Long repId;
}

public class RegistrationServiceImpl implements RegistrationService {
    private SalesRepRepository salesRepRepo;
    private UserRepository userRepo;
  
    public User register(String name, String phone, String address) 
      throws ValidationException {
        // 校验逻辑
        if (name == null || name.length() == 0) {
            throw new ValidationException("name");
        }
        if (phone == null || !isValidPhoneNumber(phone)) {
            throw new ValidationException("phone");
        }
        // 此处省略address的校验逻辑
        // 取电话号里的区号，然后通过区号找到区域内的SalesRep
        String areaCode = null;
        String[] areas = new String[]{"0571", "021", "010"};
        for (int i = 0; i < phone.length(); i++) {
            String prefix = phone.substring(0, i);
            if (Arrays.asList(areas).contains(prefix)) {
                areaCode = prefix;
                break;
            }
        }
        SalesRep rep = salesRepRepo.findRep(areaCode);
        // 最后创建用户，落盘，然后返回
        User user = new User();
        user.name = name;
        user.phone = phone;
        user.address = address;
        if (rep != null) {
            user.repId = rep.repId;
        }
        return userRepo.save(user);
    }
  
    private boolean isValidPhoneNumber(String phone) {
        String pattern = "^0[1-9]{2,3}-?\\d{8}$";
        return phone.matches(pattern);
    }
}
```



其中地推业务员、用户本身自带 ID 属性，属于 Entity（实体），而注册系统属于 Application Service（应用服务），这几个概念已经有存在。

> 实体：拥有唯一标识符，且标识符在历经各种状态变更后仍能保持一致。

但是发现 **电话号这个概念却完全被隐藏到了代码之中**。我们可以问一下自己，取电话号的区号的逻辑是否属于用户（用户的区号？）？是否属于注册服务（注册的区号？）？如果都不是很贴切，那就说明这个逻辑应该属于一个独立的概念。所以这里引入我们第一个原则：

- **Make Implicit Concepts Explicit 将隐性的概念显性化**

原来电话号仅仅是用户的一个参数，属于隐形概念，但实际上电话号的区号才是真正的业务逻辑，而我们需要将电话号的概念显性化，通过写一个 Value Object：

```java
public class PhoneNumber {
  
    private final String number;
  
    public String getNumber() {
        return number;
    }
  
    public PhoneNumber(String number) {
        if (number == null) {
            throw new ValidationException("number不能为空");
        } else if (isValid(number)) {
            throw new ValidationException("number格式错误");
        }
        this.number = number;
    }
  
    public String getAreaCode() {
        for (int i = 0; i < number.length(); i++) {
            String prefix = number.substring(0, i);
            if (isAreaCode(prefix)) {
                return prefix;
            }
        }
        return null;
    }
  
    private static boolean isAreaCode(String prefix) {
        String[] areas = new String[]{"0571", "021", "010"};
        return Arrays.asList(areas).contains(prefix);
    }
  
    public static boolean isValid(String number) {
        String pattern = "^0?[1-9]{2,3}-?\\d{8}$";
        return number.matches(pattern);
    }
}
```

**这里面有几个很重要的元素**：

1. 通过 `private final String number` 确保 `PhoneNumber` 是一个（Immutable）Value Object。（一般来说 VO 都是 Immutable 的，这里只是重点强调一下）
2. 校验逻辑都放在了 constructor 里面，确保只要 `PhoneNumber` 类被创建出来后，一定是校验通过的。
3. 之前的 `findAreaCode` 方法变成了 `PhoneNumber` 类里的 `getAreaCode` ，突出了 `areaCode` 是 `PhoneNumber` 的一个计算属性。

这样做完之后，我们发现 **把 `PhoneNumber` 显性化之后，其实是生成了一个 Type（数据类型）和一个 Class**（类）：

- Type 指我们在今后的代码里可以通过 `PhoneNumber` 去显性的标识电话号这个概念
- Class 指我们可以把所有跟电话号相关的逻辑完整的收集到一个文件里

这两个概念加起来，构造成了本文标题的 Domain Primitive（DP）。

我们看一下全面使用了 DP 之后效果：

```java
public class User {
    UserId userId;
    Name name;
    PhoneNumber phone;
    Address address;
    RepId repId;
}

public User register(
  @NotNull Name name,
  @NotNull PhoneNumber phone,
  @NotNull Address address
) {
    // 找到区域内的SalesRep
    SalesRep rep = salesRepRepo.findRep(phone.getAreaCode());
    // 最后创建用户，落盘，然后返回，这部分代码实际上也能用Builder解决
    User user = new User();
    user.name = name;
    user.phone = phone;
    user.address = address;
    if (rep != null) {
        user.repId = rep.repId;
    }
    return userRepo.saveUser(user);
}
```

我们可以看到在使用了 DP 之后，所有的数据验证逻辑和非业务流程的逻辑都消失了，剩下都是核心业务逻辑，可以一目了然。

### 2.2 让隐性的上下文显性化

假设现在要实现一个功能，让A用户可以支付 x 元给用户 B ，可能的实现如下：‍

```java
public void pay(BigDecimal money, Long recipientId) {
    BankService.transfer(money, "CNY", recipientId);
}
```

如果这个是境内转账，并且境内的货币永远不变，该方法貌似没啥问题，但如果有一天货币变更了（比如欧元区曾经出现的问题），或者我们需要做跨境转账，该方法是明显的 bug ，因为 `money` 对应的货币不一定是 CNY 。

在这个 case 里，当我们说“支付 x 元”时，除了 x 本身的数字之外，实际上是有一个隐含的概念那就是货币“元”。但是在原始的入参里，**之所以只用了 `BigDecimal` 的原因是我们认为 CNY 货币是默认的，是一个隐含的条件，但是在我们写代码时，需要把所有隐性的条件显性化，而这些条件整体组成当前的上下文**。所以 DP 的第二个原则是：

- **Make Implicit Context Explicit 将隐性的上下文显性化**

所以当我们做这个支付功能时，实际上需要的一个入参是支付金额 + 支付货币。我们可以把这两个概念组合成为一个独立的完整概念：`Money`。

```java
@Value
public class Money {
    private BigDecimal amount;
    private Currency currency;
  
    public Money(BigDecimal amount, Currency currency) {
        this.amount = amount;
        this.currency = currency;
    }
}
```

而原有的代码则变为：

```java
public void pay(Money money, Long recipientId) {
    BankService.transfer(money, recipientId);
}
```

通过将默认货币这个隐性的上下文概念显性化，并且和金额合并为 `Money` ，我们可以避免很多当前看不出来，但未来可能会暴雷的 bug。



### 2.3 封装多对象行为

前面的案例升级一下，假设用户可能要做跨境转账从 CNY 到 USD ，并且 **货币汇率随时在波动**：

```java
public void pay(Money money, Currency targetCurrency, Long recipientId) {
    if (money.getCurrency().equals(targetCurrency)) {
        BankService.transfer(money, recipientId);
    } else {
        BigDecimal rate = ExchangeService.getRate(money.getCurrency(), targetCurrency);
        BigDecimal targetAmount = money.getAmount().multiply(new BigDecimal(rate));
        Money targetMoney = new Money(targetAmount, targetCurrency);
        BankService.transfer(targetMoney, recipientId);
    }
}
```

在这个 case 里，由于 `targetCurrency` 不一定和 `money` 的 `Curreny` 一致，需要调用一个服务去取汇率，然后做计算。最后用计算后的结果做转账。

这个 case 最大的问题在于，**金额的计算被包含在了支付的服务中，涉及到的对象也有 2 个 `Currency` ，2 个 `Money` ，1 个 `BigDecimal` ，总共 5 个对象**。这种 **涉及到多个对象的业务逻辑，需要用 DP 包装掉**，所以这里引出 DP 的第三个原则：

- **Encapsulate Multi-Object Behavior 封装多对象行为**

在这个 case 里，可以将转换汇率的功能，封装到一个叫做 `ExchangeRate` 的 DP 里：

```java
@Value
public class ExchangeRate {
    private BigDecimal rate;
    private Currency from;
    private Currency to;
    public ExchangeRate(BigDecimal rate, Currency from, Currency to) {
        this.rate = rate;
        this.from = from;
        this.to = to;
    }
    public Money exchange(Money fromMoney) {
        notNull(fromMoney);
        isTrue(this.from.equals(fromMoney.getCurrency()));
        BigDecimal targetAmount = fromMoney.getAmount().multiply(rate);
        return new Money(targetAmount, to);
    }
}
```

`ExchangeRate` 汇率对象，通过封装金额计算逻辑以及各种校验逻辑，让原始代码变得极其简单：

```java
public void pay(Money money, Currency targetCurrency, Long recipientId) {
    ExchangeRate rate = ExchangeService.getRate(money.getCurrency(), targetCurrency);
    Money targetMoney = rate.exchange(money);
    BankService.transfer(targetMoney, recipientId);
}
```

## 3. DP 的使用场景

常见的 DP 的使用场景包括：

- 有格式限制的 `String`：比如 `Name`，`PhoneNumber`，`OrderNumber`，`ZipCode`，`Address `等；
- 有限制的 `Integer`：比如 `OrderId`（>0），`Percentage`（0-100%），`Quantity`（>=0）等；
- 可枚举的 `int` ：比如 `Status`（一般不用 Enum 因为反序列化问题）
- `Double` 或 `BigDecimal`：一般用到的 `Double` 或 `BigDecimal` 都是有业务含义的，比如 `Temperature`、`Money`、`Amount`、`ExchangeRate`、`Rating` 等；
- 复杂的数据结构：比如 `Map<String, List<Integer>>` 等，尽量能把 `Map` 的所有操作包装掉，仅暴露必要行为。

