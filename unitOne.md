### 我是测试

###### 文字大小

### 做有意义的区分

- 如果同一作用范围内两样不同的东西不能重名，那其意思也应该不同才对。那么这两样东西应该取不同的名字而不是以数字区分。如果以下代码参数名改为 `source` 和 `destination`，这个函数就会像样许多

  ```java
  public static void copyChars(char a1[], char a2[]){
      for(int i = 0; i < a1.length; i++){
          a2[i] = a1[i];
      }
  }
  ```

- 以数字系列命名(a1、a2、a3...)是依意义命名的对立面。这样的名称完全没有提供正确的信息

- Info和Data就像a、an和the一样，是意义含糊的废话。但有时候只要体现出有意义的区分，使用a和the这样的前缀就没错

- 废话都是冗余的。Variable一词永远不应当出现在变量名中。Table一词永远不应该出现在表名中

### 避免思维映射

例如循环中的 `i、j、k` 等单字母名称不是个好选择；读者必须在脑中将它映射为真实概念。最好用 `filter、map` 等方法代替 `for循环`



### 类名与方法名

- 类名和对象名应该是`名称或名词短语`
- 方法名应该是`动词或动词短语`

### 每个概念对应一个词

例如 `fetch`、`retrive`、 `get` 表达同一个意思，应该选定一个，然后在各个类中使用相同的方法名。

### 别用双关语

避免将同一单词用于不同的目的。同一术语用于不同概念，基本上就是双关语了。

### 使用解决方案领域名称

记住，只有程序员才会读你的代码。所以，尽管用那些计算机科学(Computer Science,CS)术语、算法名、模式名等。

### 动词与关键字

给函数取个好名字，能较好地解释函数的意图，以及参数的顺序和意图。

- 对于一元函数，函数和参数应当形成一种非常良好的动词/名词形式。

  ```java
  // good
  write(name)
  // better
  // 更具体，它告诉我们，"name"是一个"field"
  writeField(name)
  ```

- 函数名称的关键字(keyword)形式。使用这种形式，把参数的名称编码成了函数名

  ```java
  // bad
  assertEqual(expected, actual);
  
  // good 
  // 这大大减轻了记忆参数顺序的负担
  assertExpectedEqualsActual(expected, actual);
  ```



## 函数

### 短小

- 函数第一条规则是要短小。第二条规则不是要短小。越短小越好，`20行封顶`
- `if`、`else`、`while`等语句，其中的代码应该只有一行。该行大抵应该是一个函数调用语句。因为块内的函数拥有较具体说明性的名称，从而增加了文档上的价值

### 只做一件事

确保函数`不能被再拆分`

### 参数

最理想的参数数量是零，其次是一，再次是二，应尽量避免三

- **不要传递标识参数**，标识参数大声宣布函数不是做一件事。如果标识为 `true` 将会这样，标识为 `false` 则会那样

- 二元函数：有些时候两个参数正好。例如 `Point p = new Point(0, 0)`;因为点天生拥有两个参数。但大多数情况下应该尽量利用一些机制将二元函数转换成一元函数。例如，把writeField 方法写成outputStream的成员之一

  ```
  // bad
  writeField(outputStream, name);
  // good
  outputStream.writeFiled(name);
  ```

- 参数对象：如果函数看起来需要两个、三个、或三个以上参数，就说明其中一些应该封装为类了

  ```
  // bad
  Circle makeCircle(double x, double y, double, radius);
  // good
  Circle makeCircle(Point center, double radius);
  ```

  从参数封装成对象，从而减少参数数量，看起来像是在作弊，但实则并非如此。当一组参数被共同传递，就像上例中的x和y那样，往往就是该有自己名称的某个概念的一部分

### 无副作用

- 确保函数功能就像函数名描述的一样，不要做函数名描述以外的事情。应该为起一个更能描述函数功能的函数名

  ```
  public UserValidator {
      private Cyptographer cyptographer;
      
      public boolean checkPassword(String userName, String password) {
          User user = UserGateway.findByName(userName);
          if(user != User.NULL) {
              String codePhrase = user.getPhraseEncodedByPassword();
              String phrase = cyptographer.decrypt(codePhrase, password);
              if("Valid Password".equals(phrase)) {
                  // 副作用在于对这个调用
                  // checkPassword函数，顾名思义，就是用来检查密码。该名称并未暗示它会
                  // 初始化该次会话。可以重命名为 checkPasswordAndIntializeSession
                  Session.initialize();
                  return true;
              }
          }
          return false;
      }
  }
  ```

- 普通而言，应该避免使用输出参数，如果函数必须要修改某种状态，就修改所属对象的状态

  ```java
  // bad
  // 读者会弄不清s是输入参数还是输出参数
  // 也会弄不清这函数是把s添加到什么东西后面，还是把什么东西添加到s后面
  appendFooter(s); 
  // 函数签名
  public void appendFooter(StringBuffer report){}
  
  // good
  report.appendFooter()
  
  ```

```java
s
```

