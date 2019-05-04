##第一章 整洁代码

 怎样是整洁的代码？

Bjarne Stroustrup（C++发明者） 说：

  “我喜欢优雅和高效的代码，代码逻辑应当直接了当，叫缺陷难以隐藏；尽量减少依赖关系，使之便于维护；依据某种分层战略完善错误处理代码；性能调至最优，省得引诱别人做没有必要的优化，搞出一堆混乱来，整洁的代码之做好一件事。”

Ron Jeffries 对整洁代码的理解：

  1.能通过所有的测试。

  2.没有重复代码。

  3.体现系统中的全部设计理念。

  4.包含尽量少的实体、比如类、方法、函数等。

 “在以上诸项中，我最在意的是代码重复。如果一段代码重复出现，就表示某种想法未在代码中得到良好的提现。我会尽力去找出到底那是什偶么，然后在尽力的更清晰的表达出来。”

##第二章 有意义的命名



 1.名副其实。 说起来很简单。选个好名字需要花时间，但省下的时间比花掉的多。注意命名，一旦有好的命名，就换掉旧的。

```JAVA
 int d;// 消失的时间，以日计。
 int elapsedTimeInDays; 
```

2.如果同一作用范围内两样不同的不能重名，那其意思也应该不同。那么这两样东西应该取不同的名字而不是以数字区分。以下代码参数名改为 `source` 和 `destination`，这个函数就会好很多

```java
public static void copyChars(char a1[], char a2[]){
    for(int i = 0; i < a1.length; i++){
        a2[i] = a1[i];
    }
}
```

3.以数字系列命名(a1、a2、a3...)是依意义命名的对立面。这样的名称完全没有提供正确的信息

4.Info和Data就像a、an和the    一样，是意义含糊的废话。但有时候只要体现出有意义的区分，使用a和the这样的前缀就没错

5.废话都是冗余的。Variable一词永远不应当出现在变量名中。Table一词永远不应该出现在表名中

### 避免思维映射

例如循环中的 `i、j、k` 等单字母名称不是个好选择；读者必须在脑中将它映射为真实概念。最好用 `filter、map` 等方法代替 `for循环`



### 类名与方法名

- 类名和对象名应该是`名称或名词短语如`Customer` 、`WikiPage`、 `AddressParser`,避免使用`Manager`、 `Processor` 、`Data` 、`Info` 模糊的类名，也不应当是动词。
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
  // 好的命名
  write(name)
  // 更好的命名
  // 更具体，它告诉我们，"name"是一个"field"
  writeField(name)
  ```

- 函数名称的关键字(keyword)形式。使用这种形式，把参数的名称编码成了函数名

  ```java
  assertEqual(expected, actual);
  
  // 这大大减轻了记忆参数顺序的负担
  assertExpectedEqualsActual(expected, actual);
  ```



## 第三章 函数

### 短小

- 函数第一条规则是要短小。第二条规则不是要短小。越短小越好，`20行封顶`
- `if`、`else`、`while`等语句，其中的代码应该只有一行。该行大抵应该是一个函数调用语句。因为块内的函数拥有较具体说明性的名称，从而增加了文档上的价值

### 只做一件事

确保函数`不能被再拆分`

### 参数

函数参数：最理想的参数数量是零（零参数函数），其次是一（单参数函数），再次是二（双参数函数），应尽量避免三（三参数函数），有足够特殊的理由才能使用三个以上参数（多参数函数）

- **不要传递标识参数**，标识参数大声宣布函数不是做一件事。如果标识为 `true` 将会这样，标识为 `false` 则会那样

- 二元函数：有些时候两个参数正好。例如 `Point p = new Point(0, 0)`;因为点天生拥有两个参数。但大多数情况下应该尽量利用一些机制将二元函数转换成一元函数。例如，把writeField 方法写成outputStream的成员之一

  ```JAVA
  writeField(outputStream, name);
  // 修改为
  outputStream.writeFiled(name);
  ```
  
- 参数对象：如果函数看起来需要两个、三个、或三个以上参数，就说明其中一些应该封装为类了

  ```JAVA
  // 不好的
  Circle makeCircle(double x, double y, double, radius);
  // 好的
  Circle makeCircle(Point center, double radius);
  ```

  从参数封装成对象，从而减少参数数量，看起来像是在作弊，但实则并非如此。当一组参数被共同传递，就像上例中的x和y那样，往往就是该有自己名称的某个概念的一部分

### 无副作用

- 确保函数功能就像函数名描述的一样，不要做函数名描述以外的事情。应该为起一个更能描述函数功能的函数名

  ```JAVA
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
                  // 可以重命名为 checkPasswordAndIntializeSession
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
  // 不好的
  // 读者会弄不清s是输入参数还是输出参数
  // 也会弄不清这函数是把s添加到什么东西后面，还是把什么东西添加到s后面
  appendFooter(s); 
  // 函数签名
  public void appendFooter(StringBuffer report)
  
  // 好的方式
  report.appendFooter()
  ```
  
- 使用异常替代返回的错误码，即使用try/catch，但是错误处理就是一件事情，只做一件事情

```java
if (deletePage(page) == E_OK) {
if (registry.deleteReference(page.name) == E_OK) {
    if (configKeys.deleteKey(page.name.makeKey()) == E_OK) {
         logger.log("page delete");
    } else {
        logger.log("configKeys not delete");
    }
} else {
       logger.log("deleteReference from registry failed");
}
} else {
    logger.log("delete failed");
    return E_ERROR;
}
//调整为：
try {
    deletePage(page);
    registry.deleteReference(page.name);
    configKeys.deleteKey(page.name.makeKey();
} catch(Exception e) {
    logger.log(e.getMessage());
}
//进一步模块化：
public void delete(page) {
    try {
        deletePageAndAllReferences(page);
    } catch(Exception e) {
        logError(e);
    }
}
private void deletePageAndAllReferences(Page page) throws Exception {
    deletePage(page);
    registry.deleteReference(page.name);
    configKeys.deleteKey(page.name.makeKey();
}
private void logError(e) {
    logger.log(e.getMessage());
}
```



```java
package fitnesse.html;  
 
import fitnesse.responders.run.SuiteResponder;  
import fitnesse.wiki.*;  
 
public class SetupTeardownIncluder {  
  private PageData pageData;  
  private boolean isSuite;  
  private WikiPage testPage;  
  private StringBuffer newPageContent;  
  private PageCrawler pageCrawler;  
 
  public static String render(PageData pageData) throws Exception {  
    return render(pageData, false);  
  }  
 
  public static String render(PageData pageData, boolean isSuite)  
    throws Exception {  
    return new SetupTeardownIncluder(pageData).render(isSuite);  
  }  
  private SetupTeardownIncluder(PageData pageData) {  
    this.pageData = pageData;  
    testPage = pageData.getWikiPage();  
    pageCrawler = testPage.getPageCrawler();  
    newnewPageContent = new StringBuffer();  
  }  
 
  private String render(boolean isSuite) throws Exception {  
    this.isSuite = isSuite;  
    if (isTestPage())  
      includeSetupAndTeardownPages();  
    return pageData.getHtml();  
  }  
 
  private boolean isTestPage() throws Exception {  
    return pageData.hasAttribute("Test");  
  }  
 
  private void includeSetupAndTeardownPages() throws Exception {  
    includeSetupPages();  
    includePageContent();  
    includeTeardownPages();  
    updatePageContent();  
  }  
 
  private void includeSetupPages() throws Exception {  
    if (isSuite)  
      includeSuiteSetupPage();  
    includeSetupPage();  
  }  
 
  private void includeSuiteSetupPage() throws Exception {  
    include(SuiteResponder.SUITE_SETUP_NAME, "-setup");  
  }  
 
  private void includeSetupPage() throws Exception {  
    include("SetUp", "-setup");  
  }  
 
  private void includePageContent() throws Exception {  
    newPageContent.append(pageData.getContent());  
  }  
 
  private void includeTeardownPages() throws Exception {  
    includeTeardownPage();  
    if (isSuite)  
      includeSuiteTeardownPage();  
  }  
 
  private void includeTeardownPage() throws Exception {  
    include("TearDown", "-teardown");  
  }  
 
  private void includeSuiteTeardownPage() throws Exception {  
    include(SuiteResponder.SUITE_TEARDOWN_NAME, "-teardown");  
  }  
 
  private void updatePageContent() throws Exception {  
    pageData.setContent(newPageContent.toString());  
  }  
 
  private void include(String pageName, String arg) throws Exception {  
    WikiPage inheritedPage = findInheritedPage(pageName);  
    if (inheritedPage != null) {  
      String pagePathName = getPathNameForPage(inheritedPage);  
      buildIncludeDirective(pagePathName, arg);  
    }  
  }  
 
  private WikiPage findInheritedPage(String pageName) throws Exception {  
    return PageCrawlerImpl.getInheritedPage(pageName, testPage);  
  }  
 
  private String getPathNameForPage(WikiPage page) throws Exception {  
    WikiPagePath pagePath = pageCrawler.getFullPath(page);  
    return PathParser.render(pagePath);  
  }  
 
  private void buildIncludeDirective(String pagePathName, String arg) {  
    newPageContent  
    .append("\n!include ")  
    .append(arg)  
    .append(" .")  
    .append(pagePathName)  
    .append("\n");  
  }  
} 
```



##第四章 注释



注释不能美化糟糕的代码，尽管有时也需要注释，我们也该多花心思尽量减少注释量

- 注释会撒谎。
- 注释存在的越久，就离其所描述的代码越远，越来越变得全然错误。（原因很简单，程序员不能坚持维护注释）
- 很多时候，简单到只需要创建一个描述与注释所言同一事物的函数即可。
- 好注释——唯一真正好的注释是你想办法不去写的注释

##### 好注释

1. 法律信息
2. 提供信息的注释
3. 对意图的解释
4. 阐释
5. 警示
6. TODO注释（一种程序员认为应该做，但由于某些原因目前还没做的工作）
7. 放大
8. 公共API中的Javadoc

#####坏注释

1. 喃喃自语
2. 多余的注释
3. 误导性注释
4. 循规式注释
5. 日志性注释
6. 废话注释
7. 可怕的废话
8. 能用函数或变量时就别用注释
9. 位置标记
10. 括号后面的注释（尽管这对于含有深度嵌套结构的长函数可能有意义，但只会给我们更愿意编写的短小、封装的函数带来混乱。如果你发现自己想标记右括号，其实应该做的是缩短函数）
11. 归属与署名
12. 注释掉的代码
13. HTML注释
14. 非本地信息
15. 信息过多
16. 不明显的联系
17. 函数头
18. 非公共代码中的Javadoc