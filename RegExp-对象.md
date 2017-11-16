RegExp 对象表示正则表达式，它是对字符串执行模式匹配的强大工具。

**直接量语法**
/pattern/attributes

**创建 RegExp 对象的语法：**
new RegExp(*pattern*, *attributes*);

**参数**
参数 *pattern* 是一个字符串，指定了正则表达式的模式或其他正则表达式。
参数 *attributes* 是一个可选的字符串，包含属性 "g"、"i" 和 "m"，分别用于指定全局匹配、区分大小写的匹配和多行匹配。ECMAScript 标准化之前，不支持 m 属性。如果 *pattern* 是正则表达式，而不是字符串，则必须省略该参数。

**返回值**
一个新的 RegExp 对象，具有指定的模式和标志。如果参数 *pattern* 是正则表达式而不是字符串，那么 RegExp() 构造函数将用与指定的 RegExp 相同的模式和标志创建一个新的 RegExp 对象。
如果不用 new 运算符，而将 RegExp() 作为函数调用，那么它的行为与用 new 运算符调用时一样，只是当 *pattern* 是正则表达式时，它只返回 *pattern*，而不再创建一个新的 RegExp 对象。

**抛出**
SyntaxError - 如果 *pattern* 不是合法的正则表达式，或 *attributes* 含有 "g"、"i" 和 "m" 之外的字符，抛出该异常。
TypeError - 如果 *pattern* 是 RegExp 对象，但没有省略 *attributes* 参数，抛出该异常。

**修饰符**
修饰符          描述
i              执行对大小写不敏感的匹配。
g             执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。

*声明一：*
/表达式/修饰符
*声明二：*
new PegExp(表达式 , 修饰符)

**方括号**
方括号用于查找某个范围内的字符：

![方括号.png](http://upload-images.jianshu.io/upload_images/1947234-3dcfceb071606bb1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**元字符**
元字符（Metacharacter）是拥有特殊含义的字符：

![元字符.png](http://upload-images.jianshu.io/upload_images/1947234-123a97ffb123ecf5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**量词**

![量词.png](http://upload-images.jianshu.io/upload_images/1947234-6e5147e2b37eeca1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**RegExp 对象属性**

![RegExp 对象属性png](http://upload-images.jianshu.io/upload_images/1947234-6f44c2f953050e74.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**RegExp 对象方法**

![RegExp 对象方法.png](http://upload-images.jianshu.io/upload_images/1947234-d50aff44cac93253.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**支持正则表达式的 String 对象的方法**

![支持正则表达式的 String 对象的方法.png](http://upload-images.jianshu.io/upload_images/1947234-c0087b7d6370465d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
