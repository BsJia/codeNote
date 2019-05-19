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

#####     好注释

1. 法律信息

2. 提供信息的注释

3. 对意图的解释

4. 阐释

5. 警示

6. TODO注释（一种程序员认为应该做，但由于某些原因目前还没做的工作）

7. 放大

8. 公共API中的Javadoc

   

   #### 坏注释

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



##第五章 格式

写代码应该保持良好的格式，应该选一套管理代码格式的规则，然后贯彻这个规则。团队中应该达成一致，采用一套简单的代码格式规则。

##### 格式的目的

先明确一下代码格式非常重要不可忽略，好的代码格式能帮助我们更好的沟通。

##### 垂直格式

文件尺寸与类尺寸及其相关。短文件比长文件更容易接受。

##### 向报纸学习

自上而下阅读，有个头条告知主题，好让阅读者决定是否继续阅读下去、第一段应该是故事的大纲、给出粗线条的概述，隐藏了故事的细节，名称应该一目了然、细节应该逐层展开。应短小精悍。

#####垂直方向区分

每组代码用空白行区分出来，空白行是一条线索，用来标识出新的独立概念，目光会停留在空白行之后那一行



```C#
public ActionResult GetList()
        {
            ITopClient client = new TopClientDefault();
            Dictionary<string, string> dic = new Dictionary<string, string>();
            string companyID = this.CompanyID;
  
            int pageIndex = WebUtil.GetFormValue<int>("PageIndex", 1);
            int pageSize = WebUtil.GetFormValue<int>("PageSize", 10);
            string tabeName = WebUtil.GetFormValue<string>("TabName", string.Empty);

            dic.Add("CompanyID", companyID);
            dic.Add("PageIndex", pageIndex.ToString());
            dic.Add("PageSize", pageSize.ToString());
            dic.Add("TabName", tabeName);

            string result = client.Execute(SequenceApiName.SequenceApiName_GetOrderList,dic);
            return Content(result);
        }
```

```C#
public ActionResult GetList()
        {
            ITopClient client = new TopClientDefault();
            Dictionary<string, string> dic = new Dictionary<string, string>();
            string CompanyID = this.CompanyID;
            int PageIndex = WebUtil.GetFormValue<int>("PageIndex", 1);
            int PageSize = WebUtil.GetFormValue<int>("PageSize", 10);
            string TabName = WebUtil.GetFormValue<string>("TabName", string.Empty);
            dic.Add("CompanyID", CompanyID);
            dic.Add("PageIndex", PageIndex.ToString());
            dic.Add("PageSize", PageSize.ToString());
            dic.Add("TabName", TabName);
            string result = client.Execute(SequenceApiName.SequenceApiName_GetOrderList,dic);
            return Content(result);
        }
```

第一个例子中空白行显示了垂直方向的分割作用。第二个例子中就有些乱了。

##### 垂直方向的靠近



```C#
public ActionResult Delete()
        {
            string CompanyID = WebUtil.GetFormValue<string>("CompanyID");
            List<string> list = WebUtil.GetFormObject<List<string>>("List");
  
            FinanceBillProvider provider = new FinanceBillProvider(CompanyID);
            int line = provider.Delete(list);

            DataResult result = new DataResult();
            if (line > 0)
            {
                result.Code = (int)EResponseCode.Success;
                result.Message = "删除成功";
            }
            else
            {
                result.Code = (int)EResponseCode.Exception;
                result.Message = "删除失败";
            }
            return Content(JsonHelper.SerializeObject(result));
        }
```

```C#
public ActionResult Delete()
        {
  					 //获取公司ID
            string CompanyID = WebUtil.GetFormValue<string>("CompanyID");
            //获取要删除的公司列表
            List<string> list = WebUtil.GetFormObject<List<string>>("List");
            //获取公司实体
            FinanceBillProvider provider = new FinanceBillProvider(CompanyID);
            int line = provider.Delete(list);

            DataResult result = new DataResult();
            if (line > 0)
            {
                result.Code = (int)EResponseCode.Success;
                result.Message = "删除成功";
            }
            else
            {
                result.Code = (int)EResponseCode.Exception;
                result.Message = "删除失败";
            }
            return Content(JsonHelper.SerializeObject(result));
        }
```

实例中三行注释破坏了垂直方向的靠近。第一个实例更容易看出 先获取参数 再处理参数 最后返回结果。

##### 垂直距离

关系密切的概念应该互相靠近，否则就把不同的 概念放在不同的文件中去。应该避免让读者在代码源文件中跳来跳去

###### 变量声明 

尽可能靠近使用的位置

```
private static void ReadPreFerences()
{

}
```

### 横向格式

　　应该尽力保持代码行短小。死守 80 个字符的上限有点僵化，而且也不反对代码行长度达到 100 个字符或 120 个字符。再多的话，大抵就是肆意妄为了。

##### 水平方向上的区隔与靠近

　　我们使用空格字符将彼此紧密相关的事物连接到一起，也用空格字符把相关性较弱的事物分隔开。
　　在赋值操作符周围加上空格字符，以此达到强调目的。赋值语句有两个确定而重要的要素：左边和右边。空格字符加强了分隔效果。
　　另一方面，我不在函数名和左圆括号之间加空格。这是因为函数与其参数密切相关，如果隔开，就会显得互无关系。我把函数调用括号中的参数一一隔开，强调逗号，表示参数是互相分离的。
　　空格字符的另一种用法是强调其前面的运算符。
　　乘法因子之前没加空格，因为它们具有较高优先级。加减法运算项之间用空格隔开，因为加法和减法优先级较低。

##### 水平对齐

　　对齐并没有什么用，如果有较长的列表需要做对齐处理，那么问题就是在列表的长度上而不是对齐上。

##### 缩进

　　源文件是一种继承结构，而不是一种大纲结构。其中的信息涉及整个文件、文件中每个类、类中的方法、方法中的代码块，也涉及代码块中的代码块。这种继承结构中的每一层级都圈出一个范围，名称可以在其中声明，而声明和执行语句也可以在其中解释。
　　要让这种范围式继承结构可见，我们依源代码行在继承结构中的位置对源代码行做缩进处理。在文件顶层的语句，例如大多数的类声明，根本不缩进。类中的方法相对该类缩进一个层级。方法的实现相对方法声明缩进一个层级。代码块的实现相对于其容器代码块缩进一个层级，以此类推。

#### 空范围

　　有时，while 或 for 语句的语句体为空，尽量不要使用。如果无法避免，就确保空范围体的缩进，用括号包围起来。

### 团队规则

　　记住，好的软件系统是由一系列读起来不错的代码文件组成的。它们需要拥有一致和顺畅的风格。读者要能确信，他们在一个源文件中看到的格式风格在其他文件中也是同样的用法。绝对不要用各种不同的风格来编写源代码，这样会增加其复杂度

```java 
public class CodeAnalyzer
{
    private int lineCount;
    private int maxLineWidth;
    private int widestLineNumber;
    private LineWidthHistogram lineWidthHistogram;
    private int totalChars;
 
    public CodeAnalyzer()
    {
        lineWidthHistogram = new LineWidthHistogram();
    }
 
    public static List<File> FindFiles(File parentDirectory)
    {
        List<File> files = new ArrayList<File>();
        findJavaFiles(parentDirectory, files);
        return files;
    }
 
    private static void findJavaFiles(File parentDirectory, List<File> files)
    {
        for (File file : parentDirectory.listFiles())
        {
            if (file.getName().endsWith(".java"))
                files.add(file);
            else if (file.isDirectory())
                findJavaFiles(file, files);
        }
    }
 
    public void analyzeFile(File javaFile) throws Exception
    {
        BufferedReader br = new BufferedReader(new FileReader(javaFile));
    	String line;
    	while ((line = br.readLine()) != null)
    	measureLine(line);
    }
 
    private void measureLine(String line)
    {
        lineCount++;
        int lineSize = line.length();
        totalChars += lineSize;
        lineWidthHistogram.addLine(lineSize, lineCount);
        recordWidestLine(lineSize);
    }
 
    private void recordWidestLine(int lineSize)
    {
        if (lineSize > maxLineWidth)
        {
            maxLineWidth = lineSize;
            widestLineNumber = lineCount;
        }
    }
 
    public int getLineCount()
    {
        return lineCount;
    }
 
    public int getMaxLineWidth()
    {
        return maxLineWidth;
    }
 
    public int getWidestLineNumber()
    {
        return widestLineNumber;
    }
 
    public LineWidthHistogram getLineWidthHistogram()
    {
        return lineWidthHistogram;
    }
 
    public double getMeanLineWidth()
    {
        return (double)totalChars / lineCount;
    }
 
    public int getMedianLineWidth()
    {
        Integer[] sortedWidths = getSortedWidths();
        int cumulativeLineCount = 0;
        for (int width : sortedWidths)
        {
            cumulativeLineCount += lineCountForWidth(width);
            if (cumulativeLineCount > lineCount / 2)
                return width;
        }
        throw new Error("Cannot get here");
    }
 
    private int lineCountForWidth(int width)
    {
        return lineWidthHistogram.getLinesforWidth(width).size();
    }
 
    private Integer[] getSortedWidths()
    {
        Set<Integer> widths = lineWidthHistogram.getWidths();
        Integer[] sortedWidths = (widths.toArray(new Integer[0]));
        Arrays.sort(sortedWidths);
        return sortedWidths;
    }
}
```



##第六章 对象和数据结构

##### 数据抽象

看以下代码片段的区别。每段代码都表示笛卡尔平面上的一个点。不过，其中之一暴露了其实现，而另一个则完全隐藏了其实现。

 具象

```c#
public class Point {
  public double x;
  public double y;
}
```

抽象

```JAVA
public interface Point {
    double getX();
    double getY();
    void setCartesian(double x, double y); 
    double getR();
    double getTheta();
    void setPolar(double r, double theta);
}
```

2漂亮之处在于，你不知道该实现会是在矩形坐标系中还是在极坐标系中。可能两个都不是！然而，该接口还是明白无误地呈现了一种数据结构。
不过它呈现的不止一种数据结构，还固定了一套存取策略。你可以单独读取某个坐标，但必须通过一次原子操作设定所有坐标。
而1 则非常清楚地是在矩形坐标系中实现，并要求我们单个操作哪些坐标，这就暴露了实现。实际上，即便变量都是私有，而且我们也通过变量取值器和赋值器使用变量，其实现仍然暴露了。
隐藏实现并非只是在变量之间放上一个函数层那么简单。隐藏实现关乎抽象！类并不简单地用取值器和赋值器将其变量推向外间，而是暴露抽象接口，以便用户无需了解数据结构的实现就能操作数据本体。

##### 数据、对象的反对称性

上面的例子展现了对象与数据结构之间的差异。对象把数据隐藏于抽象之后，暴露操作数据的函数。数据结构暴露其数据，没有提供有意义的函数。它们是对立的。这种差异貌似微小，但却有深远的意义。

###### 过程式形状代码



```java
public class Square {
       public Point topLeft;
       public double side;
}

public class Rectangle {
    public Point topLeft;
    public double height;
    public double width;
}

public class Circle {
    public Point center;
    public double radius;
}

public double area(Object shape) throws NoSuchShapeException {
    if (shape instanceOf Square) {
        Square s = (Square) shape;
        return s.side * s.side;
    } else if (shape instanceOf Rectangle) {
        Rectangle r = (Rectangle) shape;
        return r.width * r.height;
    } else if (shape instanceOf Circle) {
        Circle c = (Circle) shape;
        return PI * c.radius * c.radius;
    }
    
    throw new NoSuchShapeException();
}
```




上面代码展示了面向过程的代码样式。Geometry类操作三个形状类，而形状类都是简单的数据结构，没有任何行为。所有行为都在Geometry类中。
面向对象程序员可能会对此嗤之以鼻，抱怨说这是过程式代码——他们大概是对的，但是这种嘲笑并不完全正确。试想一下，如果给Geometry类添加一个primeter()函数会怎样。那么形状类根本不会因此而受影响！另一方面，如果添加一个形状类，就得修改Geometry中的所有函数来处理它。注意，这两种情形也是直接对立的。
下面来看看面向对象方案。这里area()方法是多态的，不需要Geometry类。所以，如果添加一个新的形状，现有的函数一个也不会受影响，而添加新函数时所有的形状都得修改。


​    
```java
public class Square implements Shape {
    private Point topLeft;
    private double side;
		public double area() {
    	return side * side;
		}
}
    
```
```java
public class Rectangle implements Shape {
    private Point topLeft;
    private double height;
    private double width;
public double area() {
    return width * height;
}
}
```


```java
public class Circle implements Shape {
    private Point center;
    private double radius;
    public final double PI = 3.141592653589793;
public double area() {
    return PI * radius * radius;
  }
}
```



两种定义是截然对立的。这说明了对象与数据结构之间的二分原理：

过程式代码（使用数据结构的代码）便于在不改动既有数据结构的前提下添加新函数。面向对象代码便于在不改动既有函数的前提下添加新类。

反过来也说得通：

过程式代码难以添加新数据结构，因为必须修改所有函数。面向对象代码难以添加新函数，因为必须修改所有类。

所以，对于面向对象较难的事，对于过程式代码却较容易，反之亦然！
一切都是对象只是一个传说！分情况来处理采用面向对象方式还是面向过程的写法。
#### 迪米特法则(The law of Demeter)

迪米特法则又叫最少知道原则（Least Knowledge Principle，简写LKP），就是说一个对象应当对其他对象又尽可能少的了解，不和陌生人说话。
模块不应该了解它所操作的对象的内部情形。如上节所见，对象隐藏数据，暴露操作。这意味着对象不应该通过存取器暴露其内部结构，因为这样更像是暴露而非隐藏其内部结构。
更准确地说，迪米特法则认为，类C的方法f只应该调用一下对象的方法：

C
由f创建的对象
作为参数传递给f的对象
由C的实体变量持有的对象

方法不应调用由任何函数返回的对象的方法。换言之，只跟朋友谈话，不和陌生人说话。
下列代码违反了迪米特法则，因为它调用了getOptions()返回值的getScratchDir()，又调用了getScratchDir()返回值的getAbsolutePath()方法。
final String outputDir = ctx.getOptions().getScratchDir().getAbsolutePath();

#### 火车失事

这类代码常被称作火车失事。最好做如下拆分：

```java 
Options opts = ctx.getOptions();
File scratchDir = opts.getScratchDir();
final String outputDir = scratchDir.getAbsolutePath();
```

上述代码，如果ctx、Options、ScratchDir是对象，则它们的内部数据结构应当隐藏而不暴露，其违反了迪米特法则。如果ctx、Options、ScratchDir只是数据结构，没有任何行为，则它们自然会暴露其内部结构，迪米特法则就不适用了。
如果数据结构只是简单地拥有公共变量，没有函数，而对象则拥有私有变量和公共函数，这个问题就不那么混淆了。

#### 混杂

混杂导致了混合结构的出现，一半是对象，一半是数据结构。无论出于怎么样的初衷，公共访问器及改值器都把私有变量公开化，诱导外部函数以过程式程序使用数据结构的方式使用这些变量。
这类混杂增加了添加新函数的难度，也增加了添加新数据结构的难度，应避免创造这种结构。

#### 隐藏结构

假如ctx、Options、ScratchDir是拥有真实行为的对象，我们改如何改造这个呢？
两种方案来获取临时目录的绝对路径：

```java
ctx.getAbsolutePathOfScratchDirectoryOption();

或

ctx.getScratchDirectoryOption().getAbsolutePath();
```





第一种方案可能导致ctx对象中方法的暴露。第二种方案是在假设ggetScratchDirectoryOption返回一个数据结构而非对象。两种方案感觉都不好。
如果ctx是一个对象，就应该要求它做点什么，而不是要求它给出内部情形。那为何我们还要得到临时目录的绝对路径呢？  例子中源代码实际上是通过取得临时目录绝对路径来创建制定名称的临时文件。
所以，直接让ctx对象来做这件事如何？
BufferedOutputStream bos = ctx.createScrathFileStream(classFileName);

这样ctx隐藏了其内部数据结构，防止当前函数因浏览它不该知道的对象而违反了迪米特法则。

#### 数据传送对象

最为精炼的数据结构，是一个只有公共变量、没有函数的类。这种数据结构有时被称为数据传送对象，或DTO（Data Transfer Objects）。
这种结构对象在用于数据库通信、或解析套接字传递的消息之类场景中，非常有用。
对这种结构经常会封装为“bean”结构，豆结构拥有赋值器和取值器操作的私有变量。这种半封装让某些OO纯化论者感觉舒服些，不过通常并没有其他好处。

##### 总结

对象暴露行为，隐藏数据。便于添加新对象类型而无需修改现有行为，同时也难以在既有对象中添加新行为。数据结构暴露数据，没有明显的行为。便于向既要数据结构添加新行为，同时也难以向既有函数添加新数据的结构。
我们应该根据手边工作的性质，灵活的选择使用对象还是数据结构



### 第七章 错误处理

##### 错误处理不要搞乱代码逻辑

##### 使用异常而不是返回码

Bad code

```java
public class DeviceController {
    ...
    public void sendShutDown() {
        DeviceHandle handle = getHandle(DEV1);
        // Check the state of the device
        if (handle != DeviceHandle.INVALID) {
            // Save the device status to the record field
            retrieveDeviceRecord(handle);
            // If not suspended, shut down
            if (record.getStatus() != DEVICE_SUSPENDED) {
                pauseDevice(handle);
                clearDeviceWorkQueue(handle);
                closeDevice(handle);
            } else {
                logger.log("Device suspended. Unable to shut down");
            }
        } else {
            logger.log("Invalid handle for: " + DEV1.toString());
        }
    }
    ...
}
```



Good code

遇到错误最好抛出异常，这样调用代码也会非常整洁

```JAVA
public class DeviceController {
    ...
    public void sendShutDown() {
        try {
            tryToShutDown();
        } catch (DeviceShutDownError e) {
            logger.log(e);
        }
    }
    private void tryToShutDown() throws DeviceShutDownError {
        DeviceHandle handle = getHandle(DEV1);
        DeviceRecord record = retrieveDeviceRecord(handle);
        pauseDevice(handle);
        clearDeviceWorkQueue(handle);
        closeDevice(handle);
    }
    private DeviceHandle getHandle(DeviceID id) {
        ...
        throw new DeviceShutDownError("Invalid handle for: " + id.toString());
        ...
    }
    ...
}
```

#### 推荐使用不可控异常(unchecked Exception)

目前,C#、C++、Python、Ruby都不再（或从未）支持或受检异常。因为其违反了开放\闭合原则，即如果在某层次方法里面抛出了受检异常，那么其上层除非catch住，否则都要重重抛出此异常。完全的耦合了。
所以现在的建议是：
一般应用开发仅考虑抛出unchecked Exception，关键代码库，则可考虑checked Exception

##### 给出异常发生的环境说明

#### 依调用只需要使用异常类

Bad code 

覆盖了可能 抛出的所有异常，且包含重复代码

```JAVA
ACMEPort port = new ACMEPort(12);
try {
    port.open();
} catch (DeviceResponseException e) {
    reportPortError(e);
    logger.log("Device response exception", e);
} catch (ATM1212UnlockedException e) {
    reportPortError(e);
    logger.log("Unlock exception", e);
} catch (GMXError e) {
    reportPortError(e);
    logger.log("Device response exception");
}
finally {
    …
}
```



good code 

```java
LocalPort port = new LocalPort(12);
try {
    port.open();
}
catch (PortDeviceFailure e) {
    reportError(e);
    logger.log(e.getMessage(), e);
}
finally {
    …
}

public class LocalPort {
    private ACMEPort innerPort;
    public LocalPort(int portNumber) {
        innerPort = new ACMEPort(portNumber);
    }
    public void open() {
        try {
            innerPort.open();
        } catch (DeviceResponseException e) {
            throw new PortDeviceFailure(e);
        } catch (ATM1212UnlockedException e) {
            throw new PortDeviceFailure(e);
        } catch (GMXError e) {
            throw new PortDeviceFailure(e);
        }
    }
    …
}
```

事实上这就是 第三方打包类 ，方便了第三方代码使用，且可以更好的模拟第三方代码的测试

#### 不要返回null

不要返回null值的原因： 
（1）返回null值会导致程序中充满对null的判断；
（2）如果不注意对null的判断，则会导致很不友好的空指针异常；
针对这个问题也搜了一下，辩证的看到处理方法： 
（1）对于集合和数组作为返回值，使用长度为零的数组或者空集合，而不是null；
（2）字符串作为返回值，使用空字符串来代替null；
（3）当空对象与其他返回对象有一样的行为和意义时，使用空对象；

#### 不要传递null值

不传递null值的意思，即不要在方法调用时，将参数设置为null。这将会带来更不可预料的后果。 但是如果程序中如果却需要对null值进行判断，可以抛出InvalidArgumentException或者使用断言来处理（而不是仅仅忽略null值）

```java
List<Employee> employees = getEmployees(); if (employees != null) { 
for(Employee e : employees) {
  totalPay += e.getPay(); 
} } 

List<Employee> employees = getEmployees(); 
for(Employee e : employees) { 
   totalPay += e.getPay(); 
} 

public List<Employee> getEmployees() { 
  if( .. there are no employees .. ) 
     return Collections.emptyList(); } 
```

```java
public class MetricsCalculator {
public double xProjection(Point p1, Point p2) {
		return (p2.x – p1.x) * 1.5;
}
... }

public class MetricsCalculator {
public double xProjection(Point p1, Point p2) 
{ 
  if(p1==null||p2==null){
	throw InvalidArgumentException(
	"Invalid argument for MetricsCalculator.xProjection");
}
return (p2.x – p1.x) * 1.5; }
}

public class MetricsCalculator {
public double xProjection(Point p1, Point p2) { 
  assert p1 != null : "p1 should not be null";
  assert p2 != null : "p2 should not be null"; 
  return (p2.x – p1.x) * 1.5;
} 
}
```



##### 总结

整洁代码是可读的，但也要强固。可读和强固并不冲突。如果将错误处理隔离看待，独立于主要逻辑之外，就能写出顽固而整洁的代码。做到这一步，我们就能单独处理它，也极大地提升了代码的可维护性。

###八.边界



##### 使用第三方代码



1.第三方程序包和框架提供者追求普适性，这样就能在多个环境中工作，吸引广泛的用户

```java

Sensor s = (Sensor)sensors.get(sensorId );

//good
Map<Sensor> sensors = new HashMap<Sensor>(); ...
Sensor s = sensors.get(sensorId );

//better
public class Sensors {
private Map sensors = new HashMap();
public Sensor getById(String id) { return (Sensor) sensors.get(id);
}
//snip }
```

```java

@Test
public void testLogCreate() {
Logger logger = Logger.getLogger("MyLogger");
logger.info("hello"); }


@Test
public void testLogAddAppender() {
Logger logger = Logger.getLogger("MyLogger"); ConsoleAppender appender = new ConsoleAppender(); logger.addAppender(appender); logger.info("hello");
}


@Test
public void testLogAddAppender() {
Logger logger = Logger.getLogger("MyLogger"); logger.removeAllAppenders(); logger.addAppender(new ConsoleAppender(
new PatternLayout("%p %t %m%n"),
ConsoleAppender.SYSTEM_OUT)); logger.info("hello");
}

@Test
public void testLogAddAppender() {
Logger logger = Logger.getLogger("MyLogger"); logger.removeAllAppenders(); logger.addAppender(new ConsoleAppender(
new PatternLayout("%p %t %m%n"),
ConsoleAppender.SYSTEM_OUT)); logger.info("hello");
}

@Test
public void addAppenderWithoutStream() {
logger.addAppender(new ConsoleAppender( new PatternLayout("%p %t %m%n")));
logger.info("addAppenderWithoutStream"); }
}
```



2.我们建议不要将Map（或在边界上的其他接口）在系统中传递，把它保留在类或近亲类中，避免从API中返回边界接口，或将接口作为参数传递给公共API

学习性测试的好处不只是免费

1.学习性测试毫无成本，编写测试是获得这些知识（要使用的API）的容易而不会影响其他工作的途径

2.学习性测试确保第三方程序包按照我们想要的方式工作

##### 使用尚不存在的代码

编写我们想得到的接口，好处之一是它在我们控制之下，有助于保持客户代码更可读，且集中于它该完成的工作

###### 整洁的边界

1.边界上的改动，有良好的软件设计，无需巨大投入和重写即可进行修改

2.边界上的代码需要清晰的分割和定义了期望的测试。依靠你能控制的东西，好过依靠你控制不了的东西，免得日后受它控制

3.可以使用ADAPTER模式将我们的接口转换为第三方提供的接口

### 九.单元测试





##### TDD三定律

1.在编写能通过的单元测试前，不可编写生产代码(测试先行)

2.只可编写刚好无法通过的单元测试，不能编译也算不通过(测试一旦失败，开始写生产代码)

3.只可编写刚好足以通过当前失败测试的生产代码(老测试一旦通过，返回写新测试)

####测试先行的一大好处：

如果先写测试，必然得知道输入是什么，期望输出的是什么。这样就不断促进思考该如何得到这样的输出，即把设计代码也放入了测试前期的准备中。

#####保持测试整洁

1.脏测试等同于没测试，测试必须随生产代码的演进而修改，测试越脏，就越难修改

2.测试代码和生产代码一样重要，它需要被思考、被设计和被照料，它该像生产代码一般保持整洁

3.如果测试不能保持整洁，你就会失去它们，没有了测试，你就会失去保证生产代码可扩展的一切要素

#####整洁的测试

1.三个要素：可读性、可读性和可读性，明确、简洁还有足够的表达力

2.构造-操作-检验（BUILD-OPERATE-CHECK）模式，第一个环节构造测试数据，第二个环节操作测试数据，第三个部分检验操作是否得到期望的结果

3.守规矩的开发者也将他们的测试代码重构为更简洁和具有表达力的形式

```java
//good code
public void testGetPageHierarchyAsXml() throws Exception {
  makePages("PageOne", "PageOne.ChildOne", "PageTwo");
	submitRequest("root", "type:pages");
	assertResponseIsXML(); 
  assertResponseContains(
	"<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>" );
}
public void testSymbolicLinksAreNotInXmlPageHierarchy() throws Exception { 
  WikiPage page = makePage("PageOne");
	makePages("PageOne.ChildOne", "PageTwo");
	addLinkTo(page, "PageTwo", "SymPage"); 
  submitRequest("root", "type:pages");
	assertResponseIsXML(); 
  assertResponseContains(
	"<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>" );
	assertResponseDoesNotContain("SymPage"); 
}
```

```java
public void testGetDataAsXml() throws Exception { 
	makePageWithContent("TestPageOne", "test page");
	submitRequest("TestPageOne", "type:data");
	assertResponseIsXML();
	assertResponseContains("test page", "<Test");
  }
```



```java
//bad
Test
public void turnOnLoTempAlarmAtThreashold() throws Exception {
	hw.setTemp(WAY_TOO_COLD); 
	controller.tic();
  assertTrue(hw.heaterState()); 
  assertTrue(hw.blowerState()); 
  assertFalse(hw.coolerState());
  assertFalse(hw.hiTempAlarm()); 
  assertTrue(hw.loTempAlarm());
}
```



改进后的测试

```java
//good
@Test
public void turnOnLoTempAlarmAtThreshold() throws Exception {
wayTooCold();
assertEquals("HBchL", hw.getState()); }
```



#####每个测试一个断言

1.JUnit中每个测试函数都应该有且只有一个断言语句

```java
public void testGetPageHierarchyAsXml() throws Exception { 
	givenPages("PageOne", "PageOne.ChildOne", "PageTwo");
	whenRequestIsIssued("root", "type:pages");
	thenResponseShouldBeXML(); 
}

public void testGetPageHierarchyHasRightTags() throws Exception {
  givenPages("PageOne", "PageOne.ChildOne", "PageTwo");
	whenRequestIsIssued("root", "type:pages");
	thenResponseShouldContain(
	"<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>"
); 
}
```



2.最好的说法是单个测试中的断言数量应该最小化

3.更好一些的规则或许是每个测试函数中只测试一个概念

```java
	/**
* Miscellaneous tests for the addMonths() method. */
public void testAddMonths() {
	SerialDate d1 = SerialDate.createInstance(31, 5, 2004);
	SerialDate d2 = SerialDate.addMonths(1, d1); 
  assertEquals(30, d2.getDayOfMonth()); 		
  assertEquals(6, d2.getMonth());
  assertEquals(2004, d2.getYYYY());
	SerialDate d3 = SerialDate.addMonths(2, d1);
  assertEquals(31, d3.getDayOfMonth());
  assertEquals(7, d3.getMonth()); 
  assertEquals(2004, d3.getYYYY());
	SerialDate d4 = SerialDate.addMonths(1, SerialDate.addMonths(1, d1)); assertEquals(30, 			d4.getDayOfMonth());
	assertEquals(7, d4.getMonth());
	assertEquals(2004, d4.getYYYY());
}
```



4.最佳规则是应该尽可能减少每个概念的断言数量，每个测试函数只测试一个概念

###F.I.R.S.T

1.快速（Fast）测试应该够快

2.独立（Independent）测试应该相互独立

3.可重复（Repeatable）测试应当可在任何环境中重复通过

4.自足验证（Self-Validating）测试应该有布尔值输出，自己就能给出对错，而不需要通过看日志，比对结果等方式验证

5.及时（Timely）测试应及时编写

### 十.类



#####类的组织

1.类应该从一级变量列表开始，如果有公共静态变量，应该先出现，然后是私有静态变量，以及实体变量，很少会有公共变量

2.公共函数应该跟在变量列表之后

3.保持变量和工具函数的私有性，但并不执着于此

#####类应该短小

1.职责单一，类的名称应当描述其权责，如果无法为某个类命以精确的名称，这个类大概就太长了，类名越含混，该类越有可能拥有过多权责；类或模块应有且只有一条加以修改的理由

2.内聚性强，方法操作的变量越多，就越黏聚到类上，如果一个类的每个变量都被每个方法所使用，则该类具有最大的内聚性

```java
  public class Stack {
         private int topOfStack = 0;
         List<Integer> elements = new LinkedList<>();

          public int size() {
             return topOfStack;
         }

          public void push(int element) {
             topOfStack++;
             elements.add(element);
         }

          public int pop() throws Exception {
             if (topOfStack == 0) {
                 throw new Exception("PoppedWhenEmpty");
             }
             int element = elements.get(--topOfStack);
             elements.remove(topOfStack);
             return element;
         }
     }
```



3.保持函数和参数列表短小的策略，有时会导致为一组子集方法所用的实体变量数量增加。出现这种情况时，往往意味着至少有一个类要从大类中挣扎出来。你应当尝试将这些变量和方法分拆到两个或多个类中，让新的类更为内聚

```java


import java.util.ArrayList;

public class PrimeGenerator {
  private static int[] primes;
  private static ArrayList<Integer> multiplesOfPrimeFactors;

  protected static int[] generate(int n) {
    primes = new int[n];
    multiplesOfPrimeFactors = new ArrayList<Integer>();
    set2AsFirstPrime();
    checkOddNumbersForSubsequentPrimes();
    return primes;
  }

  private static void set2AsFirstPrime() {
    primes[0] = 2;
    multiplesOfPrimeFactors.add(2);
  }

  private static void checkOddNumbersForSubsequentPrimes() {
    int primeIndex = 1;
    for (int candidate = 3;
         primeIndex < primes.length;
         candidate += 2) {
      if (isPrime(candidate))
        primes[primeIndex++] = candidate;
    }
  }

  private static boolean isPrime(int candidate) {
    if (isLeastRelevantMultipleOfNextLargerPrimeFactor(candidate)) {
      multiplesOfPrimeFactors.add(candidate);
      return false;
    }
    return isNotMultipleOfAnyPreviousPrimeFactor(candidate);
  }

  private static boolean
  isLeastRelevantMultipleOfNextLargerPrimeFactor(int candidate) {
    int nextLargerPrimeFactor = primes[multiplesOfPrimeFactors.size()];
    int leastRelevantMultiple = nextLargerPrimeFactor * nextLargerPrimeFactor;
    return candidate == leastRelevantMultiple;
  }

  private static boolean
  isNotMultipleOfAnyPreviousPrimeFactor(int candidate) {
    for (int n = 1; n < multiplesOfPrimeFactors.size(); n++) {
      if (isMultipleOfNthPrimeFactor(candidate, n))
        return false;
      }
    return true;
  }

  private static boolean
  isMultipleOfNthPrimeFactor(int candidate, int n) {
    return
      candidate == smallestOddNthMultipleNotLessThanCandidate(candidate, n);
  }

  private static int
  smallestOddNthMultipleNotLessThanCandidate(int candidate, int n) {
    int multiple = multiplesOfPrimeFactors.get(n);
    while (multiple < candidate)
      multiple += 2 * primes[n];
    multiplesOfPrimeFactors.set(n, multiple);
    return multiple;
  }
}
```

```JAVA


public class PrimePrinter {
  public static void main(String[] args) {
    final int NUMBER_OF_PRIMES = 1000;
    int[] primes = PrimeGenerator.generate(NUMBER_OF_PRIMES);

    final int ROWS_PER_PAGE = 50;
    final int COLUMNS_PER_PAGE = 4;
    RowColumnPagePrinter tablePrinter =
      new RowColumnPagePrinter(ROWS_PER_PAGE,
                               COLUMNS_PER_PAGE,
                               "The First " + NUMBER_OF_PRIMES +
                                 " Prime Numbers");

    tablePrinter.print(primes);
  }

}
```



4.将大函数拆为许多小函数，往往也是将类拆分为多个小类的时机

#####为了修改而组织

1.在整洁的系统中，我们对类加以组织，以降低修改的风险

```java
public class Sql {

          public Sql(String table, Column[] columns) {}

          public String create() {
             return null;
         }

          public String insert(Object[] fields) {
             return null;
         }

          public String selectAll() {
             return null;
         }

          //  ...下面还有很多方法，但是没有 update
     }
```



```java
 abstract class ModifySql {

          public ModifySql(String table, Column[] columns) {}

          abstract public String generate();

          public class CreateSql extends ModifySql {

              public CreateSql(String table, Column[] columns) {
                 super(table, columns);
             }

              @Override
             public String generate() {
                 return null;
             }
         }

          public class SelectSql extends ModifySql {

              public SelectSql(String table, Column[] columns) {
                 super(table, columns);
             }

              @Override
             public String generate() {
                 return null;
             }
         }

          public class InsertSql extends ModifySql {

              public InsertSql(String table, Column[] columns) {
                 super(table, columns);
             }

              @Override
             public String generate() {
                 return null;
             }
         }

          //  ...省略若干类与方法
     }

      class Column {}
```

```java


import java.io.PrintStream;

public class RowColumnPagePrinter {
  private int rowsPerPage;
  private int columnsPerPage;
  private int numbersPerPage;
  private String pageHeader;
  private PrintStream printStream;

  public RowColumnPagePrinter(int rowsPerPage,
                              int columnsPerPage,
                              String pageHeader) {
    this.rowsPerPage = rowsPerPage;
    this.columnsPerPage = columnsPerPage;
    this.pageHeader = pageHeader;
    numbersPerPage = rowsPerPage * columnsPerPage;
    printStream = System.out;
  }
  public void print(int data[]) {
    int pageNumber = 1;
    for (int firstIndexOnPage = 0;
         firstIndexOnPage < data.length;
         firstIndexOnPage += numbersPerPage) {
      int lastIndexOnPage =
        Math.min(firstIndexOnPage + numbersPerPage - 1,
                 data.length - 1);
      printPageHeader(pageHeader, pageNumber);
      printPage(firstIndexOnPage, lastIndexOnPage, data);
      printStream.println("\f");
      pageNumber++;
    }
  }

  private void printPage(int firstIndexOnPage,
                         int lastIndexOnPage,
                         int[] data) {
    int firstIndexOfLastRowOnPage =
      firstIndexOnPage + rowsPerPage - 1;
    for (int firstIndexInRow = firstIndexOnPage;
         firstIndexInRow <= firstIndexOfLastRowOnPage;
         firstIndexInRow++) {
      printRow(firstIndexInRow, lastIndexOnPage, data);
      printStream.println("");
    }
  }

  private void printRow(int firstIndexInRow,
                        int lastIndexOnPage,
                        int[] data) {
    for (int column = 0; column < columnsPerPage; column++) {
      int index = firstIndexInRow + column * rowsPerPage;
      if (index <= lastIndexOnPage)
        printStream.format("%10d", data[index]);
    }
  }

  private void printPageHeader(String pageHeader,
                               int pageNumber) {
    printStream.println(pageHeader + " --- Page " + pageNumber);
    printStream.println("");
  }

  public void setOutput(PrintStream printStream) {
    this.printStream = printStream;
  }
}
```



2.开放-闭合原则（OCP）：类应当对扩展开放，对修改封闭

3.在理想系统中，我们通过扩展系统而非修改现有代码来添加新特性


--------------------- 
