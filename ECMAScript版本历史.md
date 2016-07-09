#### 名词解释：

**ISO**: 国际标准化组织（International Organization for Standardization，ISO）简称ISO，是一个全球性的非政府组织，是国际标准化领域中一个十分重要的组织。

**ECMA**: Ecma国际（Ecma International）是一家国际性会员制度的信息和电信标准组织。原名为欧洲计算机制造商协会ECMA（European Computer Manufacturers Association）

**ECMAScript**:  ECMAScript 是标准化组织 ECMA（Ecma International - European association for standardizing information and communication systems）发布的脚本语言规范。

**ECMA第39号技术委员会 (TC39)**: 负责制定和审核ECMA-262标准，成员由业内的大公司派出的工程师组成，目前共25个人。该委员会定期开会，所有的邮件讨论和会议记录，都是公开的。

******

**维基百科：**

版本 |  发表日期  | 与前版本的差异 
---|---|---
1 |	1997.6   |	首版
2 |	1998.6   |	格式修正，以使得其形式与ISO/IEC16262国际标准一致
3 |	1999.12 |	强大的正则表达式，更好的词法作用域链处理，新的控制指令，异常处理，错误定义更加明确，数据输出的格式化及其它改变
4 |	放弃 |	由于关于语言的复杂性出现分歧,第4版本被放弃,其中的部分成为了第5版本及Harmony的基础。
5 |	2009.12 |	新增“严格模式（strict mode）”，一个子集用作提供更彻底的错误检查,以避免结构出错。澄清了许多第3版本的模糊规范,and accommodates behaviour of real-world implementations that differed consistently from that specification。增加了部分新功能,如getters及setters,支持JSON以及在物件属性上更完整的反射。
6 | 2015.6 |	多个新的概念和语言特性。ECMAScript Harmony将会以“ECMAScript 6”发布。
7 | 工作中 | 多个新的概念和语言特性

******
\* 2004年6月Ecma组织发表了ECMA-357标准，它是ECMAScript的一个扩延，也被称为E4X（ECMAScript for XML）。

##### ECMAScript 1

1997年6月发布：

本质上与javascript 1.1 相同

只不过只不过删除了所有针对浏览器的代码并作了一些较小的改动：ECMAScript要求支持Unicode标准，而且对象也变成了平台无关的。
******
##### ECMAScript 2

1998年6月发布：

主要是编辑加工的结果。这一版的内容更新是为了与ISO/IEC-16262保持严格一致，没有作任何新增、修改或删节处理。

因此，一般不使用第2版来衡量ECMAScript实现的兼容性。
******
##### ECMAScript 3

1999年12月发布：

是对ECMAScript标准第一次真正的修改。

新增了对正则表达式、新控制语句、try-catch异常处理的支持，修改了字符处理、错误定义和数值输出等内容。

从各方面综合来看，第3版标志着ECMAScript成为了一门真正的编程语言。也成为JavaScript的通行标准，得到了广泛支持。
******
##### ECMAScript 4

**2007年10月ECMAScript 4.0版草案发布**

对3.0版做了大幅升级，预计次年8月发布正式版本。

草案发布后，由于4.0版的目标过于激进，各方对于是否通过这个标准，发生了严重分歧。

以Yahoo、Microsoft、Google为首的大公司，反对JavaScript的大幅升级，主张小幅改动；
以JavaScript创造者Brendan Eich为首的Mozilla公司，则坚持当前的草案。

**2008年7月ECMAScript 4.0发布前被废弃**

由于对于下一个版本应该包括哪些功能，各方分歧太大，争论过于激进，ECMA开会决定，中止ECMAScript 4.0的开发（即废除了这个版本）。

将其中涉及现有功能改善的一小部分，发布为ECMAScript3.1，而将其他激进的设想扩大范围，放入以后的版本，由于会议的气氛，该版本的项目代号起名为Harmony（和谐）。

会后不久，ECMAScript 3.1就改名为ECMAScript 5。

******
##### ECMAScript 5

2009年12月发布：

**ECMAScript 5.0版发布:**

Harmony项目则一分为二，
一些较为可行的设想定名为JavaScript.next继续开发，后来演变成ECMAScript 6；
一些不是很成熟的设想，则被视为JavaScript.next.next，在更远的将来再考虑推出。

TC39的总体考虑是，ECMAScript5与ECMAScript3基本保持兼容，较大的语法修正和新功能加入，将由JavaScript.next完成。
(当时，JavaScript.next指的是ECMAScript 6。第六版发布以后，将指ECMAScript 7)
该版本力求澄清第3版中的歧义，并添加了新的功能。

新功能包括：原生JSON对象、继承的方法、高级属性的定义以及引入严格模式。


2011年6月发布:

**ECMAscript 5.1版发布:**

并且成为ISO国际标准（ISO/IEC16262:2011）。到了2012年底，所有主要浏览器都支持ECMAScript 5.1版的全部功能

******
##### ECMAScript 6

2015年6月发布：

ECMAScript 6正式发布，并且更名为“ECMAScript 2015”。

这是因为TC39委员会计划，以后每年发布一个ECMAScirpt的版本，下一个版本在2016年发布，称为“ECMAScript 2016”。

从现在开始，新版本将按照ECMAScript+年份的形式发布。

S6是继S5之后的一次主要改进，语言规范由ES5.1时代的245页扩充至600页。尽管ES6做了大量的更新，但是它依旧完全向后兼容以前的版本。

ES6增添了许多必要的特性，新功能包括：模块和类以及一些实用特性，例如Maps、Sets、Promises、生成器（Generators）等。

******
参考资料：

[JavaScript语言的历史](http://javascript.ruanyifeng.com/introduction/history.html)

[ECMAScript各版本简介及特性](http://www.07net01.com/2015/08/913846.html)

[ECMAScript百度百科](http://baike.baidu.com/link?url=Zk-zXZMI-cm_qZvmhYQ0w1JpJ8R2hRVTjoEB1yODJx59CK7dhCcosaUGe7ZDpc5K82ewueYnOVxfdpzzZHWz9K)
