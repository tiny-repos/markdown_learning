***
2018-03-07 星期三
http://spec.commonmark.org
--------------------

# 引言

Markdown最初由 John Gruber (with help from Aaron Swartz) 开发,并在 2004 年以
一个语法描述的形式发布. 此语法描述 见网址
(http://daringfireball.net/projects/markdown/)

Markdown 的优点是极其易于阅读, markdown 即使没有经过转换, 也相当符合人类的
阅读习惯, 它没有各种乱七八糟的标记.

在markdown中,缩进要小心对待,这是一种代价.

John Gruber 的"权威"Markdown语法说明不够精确,其定义诸多含糊不清之处.
此文档试图给出无歧义的markdown语法规范.

# 准备活动

## 字符和行(Characters and lines)

有以下约定

任何字符序列都是有效的Markdown文档.

一个字符字符是一个Unicode代码点.(即使有些Unicode代码点并不是字符)

不考虑文档的所采用的编码,将文档视作字符序列而非字节序列.

一行是指零至多个字符组成的序列(除换行\n,回车\r),且序列后紧跟一个 行结尾 或
文档结尾.

行结尾(line ending)是指 \n 或 \r(\r后不跟\n) 或 \r\n,

空白行(blank line) 是指不含任何字符的行,或仅含空格与水平制表符的行.

下面定义字符类.

空白字符: 空格,水平制表符,换行,回车,换页(空格,\t, \r, \n, \f) 以及
linetabulation (U+000B)

空白: 一至多个空白字符序列

Unicode 空白字符: \t, \r, \n, \f 以及任何在Unicode Zs general category中的
代码点.

Unicode 空白: 一至多个Unicode字符组成的序列

非空白字符: 除空白字符之外的字符.

ASCII 标点字符: 枚举如下
    !, ", #, $, %, &, ', (, ), *, +, ,, -, ., /, :, ;,
    <, =, >, ?, @, [, \, ], ^, _, `, {, |, }, ~.

标点字符: ASCII标点字符及人何在 general Unicode categories Pc, Pd, Pe, Pf,
Pi, Po, or Ps.的代码点

## 水平制表符(tab)

一般, tab 不会扩展为空格. 但当空白用于定义块结构时, 一个tab与四个空格等价.
所以, 代码块中,可用一个tab代替四个空格的缩进.但行内的tab会原样保留.
再有,区块引用以> 标识,>后有一个可选的空格,如果有空格,则在显示效果中的空格数
要减去一,所以 > 后跟一个tab的效果相当于是两个空格.
(大意是这样,具体看原文例子)

# 区块和行内(blocks and inlines)

可以认为,文档是一个区块组成的序列.区块包括:标题,段落,分隔线,列表,区块级引用,
代码块. 其中,区块级引用和列表项可包含其它区块; 而标题,段落则包含inline.

行内结构包括:文本,链接,强调文本,代码扩展(行内代码)等等.

## 优先级

区块级结构指示符总是优先于行内结构指示符.所以
    - `item
    - item`
被解析为两个列表项,而不是一个包含code span 的列表项.

## 容器区块和叶子区块(Container blocks and Leaf blocks)

容器区块可以内嵌其它区块;而叶子区块不可以.

# 叶子区块

## 分隔线 (Thematic breaks ) <hr />

独占一行的三个*或-或_构成分隔线. 可以多于三个但不可以少. 行首可以有至多三个
空格的缩进. 中间可以穿插任意多个空白.

分隔线可以用作段落分隔符.

三个减号,在setex语法下构成标题,有此冲突时,setext标题优先级更高.

如果既可以解析为列表又可以解析为分隔线,按分隔线解析
    * item
    * * *
    * item
如果确实要创建包含分隔线的列表项, 换一个列表指示符
    - item
    - * * *
    - item

## ATX 风格的标题 --- 一至六个 #(井号) 
    # h1
    ## h2
    ###### h6

行前可以有至多三个空格的缩进;

标题文字后可以有任意多个 # 序列结尾

开头 # 序列与标题文字之间必须有至少一个空格, 除非标题为空,
当前许多实现允许省略空格,但原生ATX实现要求必须有空格.

结尾 # 序列与标题文字之间必须有至少一个空格

解析之前,标题文字首尾的一切空白都被去除.

多于六个的 # 不是标题

ATX标题不要求用标题前后用空白行分隔,ATX标题总是独占一个段落.

## setext 风格的标题 ?

连续若干行,后跟一个setext标题下划线.
若干行均为标题内容, 每行至多三个空格的缩进;

setext下划线是是指长度不为零的-或=字符序列,其前可有至多三个字符的缩进,
其尾不得的有空格.

还有许多细节方面的规则,略过.

## 缩进代码块

一个缩进代码块由一至多个以空行分隔的缩进段组成,直到遇到不足四个空格缩进的非
空白行.
一个缩进段是一个非空白行序列,序列中的每一行都有至少四个空格的缩进.
缩进代码段的内容按原文保留不变,但每行行首减去四个空格.

缩进代码段不会打断段落,要想分段,需在缩进代码段的前面使用空行.
但缩进代码段与其之后的段落之间不必使用空行.
    para1

        code
    parar2

缩进代码块可以与其他区块紧相邻而不必用空格分隔.
缩进代码块开头和末尾的空行被忽略.
缩进代码块内的markdown指示符不再具备特殊含义.

## 围栏式代码块(Fenced code blocks ) ~~~ ```

一个代码围栏(code fence)是一个包含至少三个反引号或三个波浪线的序列.
围栏代码块是指位于两对代码围栏之间的内容, 缩进量不大于三个空格.

起始代码围栏后可以添加信息,此信息不会显示,首尾空白会被剔除,信息内不得含有反
引号.
围栏代码块内首尾的空行会原样保留(这与缩进式代码块不同).

若起始代码围栏缩进了N个空格,则整个围栏式代码块,每一行的前N个(如果有那么多)
空格都被忽略.
结尾代码围栏可以缩进,缩进量不得大于三个空格.
围栏代码块会终止其前面的段落.
围栏代码块可以与其他区块直接相连而不必用空行分隔.

## HTML区块 ??

HTML区块是直接嵌入的HTML代码段.
HTML区块分为七类. 

一个start condition定义HTML区块的开始.
紧随其后的end condition 标志HTML区块的结束.(如过没有对应的end condition,
则当遇到文档尾或所在容器区块的结束时,HTML区块也结束)

HTML区块内的文本按原始HTML代码段移动.
(就是说,HTML区块内,Markdown语法是无效的.)

这七类区块标记是

1.  **Start condition:**  line begins with the string `<script`,
`<pre`, or `<style` (case-insensitive), followed by whitespace,
the string `>`, or the end of the line.\
**End condition:**  line contains an end tag
`</script>`, `</pre>`, or `</style>` (case-insensitive; it
need not match the start tag).

2.  **Start condition:** line begins with the string `<!--`.\
**End condition:**  line contains the string `-->`.

3.  **Start condition:** line begins with the string `<?`.\
**End condition:** line contains the string `?>`.

4.  **Start condition:** line begins with the string `<!`
followed by an uppercase ASCII letter.\
**End condition:** line contains the character `>`.

5.  **Start condition:**  line begins with the string
`<![CDATA[`.\
**End condition:** line contains the string `]]>`.

6.  **Start condition:** line begins the string `<` or `</`
followed by one of the strings (case-insensitive) `address`,
`article`, `aside`, `base`, `basefont`, `blockquote`, `body`,
`caption`, `center`, `col`, `colgroup`, `dd`, `details`, `dialog`,
`dir`, `div`, `dl`, `dt`, `fieldset`, `figcaption`, `figure`,
`footer`, `form`, `frame`, `frameset`,
`h1`, `h2`, `h3`, `h4`, `h5`, `h6`, `head`, `header`, `hr`,
`html`, `iframe`, `legend`, `li`, `link`, `main`, `menu`, `menuitem`,
`meta`, `nav`, `noframes`, `ol`, `optgroup`, `option`, `p`, `param`,
`section`, `source`, `summary`, `table`, `tbody`, `td`,
`tfoot`, `th`, `thead`, `title`, `tr`, `track`, `ul`, followed
by [whitespace], the end of the line, the string `>`, or
the string `/>`.\
**End condition:** line is followed by a [blank line].

7.  **Start condition:**  line begins with a complete [open tag]
or [closing tag] (with any [tag name] other than `script`,
`style`, or `pre`) followed only by [whitespace]
or the end of the line.\
**End condition:** line is followed by a [blank line].

    第一条是说 pre,script,style 三个HTML标签成对匹配.
    二三四五条说 四对: `<!--` 与 `-->`, `<?` 与 `?>`, 
                        `<!` 与 `!>` , `<![CDATA[` 与 `]]`
    六七条说 ,说的好复杂.

权威Markdown语法的HTML block规则更容易理解:
    凡是HTML block必须用空行与上下文分隔开
    HTML 标签必须成对
    标记HTML区块起始的HTML的标签不得缩进.

## 链接引用定义
    格式 [label]: url title
    其中 []: 是标志符
    label和url不可省略
    title必须用双引号或单引号或圆括号括起,
    title和url之间的空格不可省略.
    除此之外,行末不得有其它非空白字符.


    :后的空格省略(经典Markdown中不允许省略)
    title可省略    
    [lable]:, url, title 可分行,各占一行,但中间不得有空行;
    title可占多行,但同样不可有空行
    链接引用

    Both title and destination can contain backslash escapes and literal backslashes:

    链接引用中label不区分大小写
    有多个匹配时, 最前面的定义有效.

    链接引用定义不能中断段落,需以空行分隔.
    链接引用与紧随其后的区块(包括段落)可不用空行分隔.

## 段落
一系列连续的非空白行, 如果不能被解析为任何其它区块,则解析为一个段落.

整个段落首尾的空白以及各行行首的空白都会被剔除. 
也就是说, 从段落的第二行开始, 行首可以缩进任意长度,因为他们都会被剔除. 但段
落第一行至多缩进三个空格,否则将被解析为 缩进式代码块.

## 空白行
文档首尾的空白行被忽略.
区块之间的空白行被忽略, 唯一的例外是列表中用于标识松散列表的空白行.




# 容器区块
只有两种基本的容器区块: 区块引用和列表项. 列表是列表项的元容器.
## 区块引用 >
    > 前可有至多三个空格的所经
    > 后的空格可以省略
    区块引用可以嵌套
区块定义规则
    区块引用标记是指0~3个空格的缩进后跟>,再跟一个可选的空格.

    1. 基本情形: 如果若干行文本 L 购成区块序列 B, 那么,在L的每一行的行首
        添加区块引用标记 > 后所得文本就是包含 B 的引用区块.
    2. 懒惰式: 如果文本行序列 L 构成内容为 B 的引用区块, 那么删除 L 中任意
    若干行行首的区块引用标记后所得文本仍是以 B 为内容的引用区块. 但所删除的
    行必须满足: 去掉行首引用区块后,行的第一个非空白字符必须是段落续接文本,
    段落续接文本是指属于某个段落且不是段落首行的文本.
    3. 顺序: 空白行终止引用区块.
## 列表项???
无序列表标记: - + *
有序列表标记: 数字加句点 或数字加右括号)
数字是指十进制数字,且不得超过9位.

规则: 
    如果

列表标记和其后的字符之间必须有至少一个空格(至多三个)
一个列表项允许其内容有不超过一个的连续空行.
列表项如果有多行,则每一行的缩进量必须等于列表标记的宽度加上列表标记后的
空格数.


# 行内区块
--------------------
    分隔线 - _ * 

    优先级: setext heading > 分隔线 > 列表 > 缩进式代码段

    这些内容的行首可以有不多于三个的空格缩进
        ATX标题,SeText标题,分隔线


    setext风格的标题下划线尾部不可以有空格.

    围栏代码块是CommonMark中的语法,经典Markdown并无此语法.

    列表标记和其后的字符之间必须有至少一个空格.

那些结构可以打断段落? 不必使用空白行与段落分隔
    分隔线, 
    ATX 分隔的标题
    缩进代码块和紧随其后的段落
    围栏式代码块
    区块引用和之前的段落
那些结构不能打断段落?
    缩进代码块和其之前的段落
    HTML区块
    链接引用定义
    区块引用和之后的段落(因为懒惰规则)

对于分段来说, 缩进式代码块, 链接引用定义的情况相同.


setText分隔的标题, HTML区块

