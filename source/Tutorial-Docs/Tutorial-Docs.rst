文档编写教程
===============================

文档结构(Document Structure)
----------------------------------

Document
^^^^^^^^^^^^^^^^

Doctree element: document。

已解析的 reStructuredText 文档的顶级元素是 “document” 元素。初始解析后，文档元素是一个 文档片段的简单容器，由正文组成 元素、过渡和部分，但缺少文档标题 或其他书目元素。调用解析器的代码可能 选择运行一个或多个可选的解析后转换， 将文档片段重新排列为一个完整的文档，并使用 标题和可能的其他元数据元素（作者、日期等;请参阅书目字段）。

具体来说，没有办法指示文档标题和 subtitle 显式出现在 reStructuredText 中。[4]取而代之的是，一个孤独的顶级 章节标题（见以下章节）可以视为文档 标题。同样，紧随其后的单独二级部分标题 “文档标题”可以成为文档副标题。其余的 然后将这些部分抬高一两层。查看 DocTitle 转换以获取详细信息。


Section
^^^^^^^^^^^^^^^^

Doctree element: section, title

各部分通过其标题进行标识，标题标有 装饰：标题文本下方的“下划线”，或下划线和 匹配标题上方的“上划线”。下划线/上划线是 从第 1 列开始的单个重复标点符号和 形成一条线，至少延伸到标题的右边缘 发短信。[5]具体来说，下划线/上划线字符可以是任何 非字母数字可打印 7 位 ASCII 字符 [6]。当 使用上划线，则使用的长度和字符必须与 下划线。仅下划线装饰样式与 使用相同字符的下划线样式和下划线样式。可能有 是任意数量的章节标题级别，尽管一些输出 格式可能有限制（HTML 有 6 个级别）。


正文元素(Body Elements)
----------------------------------

段落(Paragraphs)
^^^^^^^^^^^^^^^^

Doctree element: paragraph

段落由没有标记的左对齐文本块组成 表示任何其他身体元素。空行分隔段落彼此之间以及来自其他身体元素。段落可以包含内联标记。

无序列表(Bullet Lists)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Doctree element: bullet_list, list_item

无序列表是以“*”、“+”、“-”、“•”、“‣”或“⁃”开头的文本块，后跟空格，是一个项目符号列表项（又名“无序”列表 项目）。列表项正文必须左对齐并相对于符号；紧跟在项目符号后面的文本确定缩进。例如::

    * this is
    * a list

    * with a nested list
    * and some subitems

      * with a nested list
      * and some subitems

    * and here the parent list continues

实际输出:

* this is
* a list

  * with a nested list
  * and some subitems

    * with a nested list
    * and some subitems

* and here the parent list continues

有序列表(Enumerated Lists)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Doctree element: enumerated_list, list_item

枚举列表（又名“有序”列表）类似于项目符号列表， 但使用枚举器而不是项目符号。枚举器由一个枚举序列成员和格式，后跟空格。可识别以下枚举序列：

- 阿拉伯数字： 1， 2， 3， ...（无上限）。
- 大写字母字符：A、B、C、...、Z。
- 小写字母字符：A、B、C、...、Z。
- 大写罗马数字：I、II、III、IV、...、MMMMCMXCIX (499)。
- 小写罗马数字：i、ii、iii、iv、...、mmmmcmxcix (499)。

此外，自动枚举器 ``“#”`` 可用于自动 枚举列表。自动枚举列表可能以显式开头枚举，用于设置序列。完全自动枚举的列表使用 阿拉伯数字，以 1 开头。

可识别以下格式类型：

- 后缀为句点：1.、A.、a.、I.、i.。
- 用括号括起来：(1)、(A)、(a)、(I)、(i)。
- 后缀为右括号：1)、A)、a)、I)、i)。

在分析枚举列表时，将在以下任何时候启动一个新列表：

- 遇到格式不相同的枚举器，并且 序列类型作为当前列表（例如“1.”，“(a)”产生两个 单独的列表）。
- 枚举器不按顺序排列（例如，“1.”、“3.”生成两个 单独的列表）。

建议第一个列表项的枚举器 序号 1（“1”、“A”、“a”、“I”或“i”）。虽然其他起始值 将被识别，它们可能不受输出格式的支持。一个 将为任何以此开头的列表生成 1 级 [INFO] 系统消息 使用非序号 1 枚举器。

使用罗马数字的列表必须以“I”/“i”或 多字符值，例如“II”或“XV”。任何其他 单字符罗马数字（“V”、“X”、“L”、“C”、“D”、“M”）将是 解释为字母表中的字母，而不是罗马数字。 同样，使用字母表的列表可能不会以 “I”/“i”，因为它们被识别为罗马数字 1。

检查每个枚举列表项的第二行的有效性。 这是为了防止普通段落被错误地 解释为列表项，当它们碰巧以文本开头时 与枚举器相同。例如，此文本被解析为 普通段落：

A. Einstein was a really
smart dude.
但是，如果该段落仅包含 一行。此文本被分析为枚举列表项：

A. Einstein was a really smart dude.
谨慎！

如果单行段落以与枚举器相同的文本开头 （“A.”、“1.”、“（b）”、“I）”等），第一个字符必须是 转义以便将该行解析为普通段落：

\A. Einstein was a really smart dude.
或者，您可以转义分隔符

A\. Einstein was a really smart dude.
或在首字母后面使用字面上的 NO-BREAK SPACE。

枚举列表的示例::

    1. Item 1 initial text.

       a) Item 1a.
       b) Item 1b.

    2. a) Item 2a.
       b) Item 2b.

    #. Item 3 initial text.

       a) Item 3a.
       b) Item 3b.

实际输出如下:

1. Item 1 initial text.

   a) Item 1a.
   b) Item 1b.

2. a) Item 2a.
   b) Item 2b.

#. Item 3 initial text.

   a) Item 3a.
   b) Item 3b.

定义列表(Definition Lists)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Doctree element: definition_list, definition_list_item, term, classifier, definition

定义列表由一个或多个定义列表项组成。每个定义列表项由术语、可选的分类器和定义组成。术语、分类器和定义之间的空行分隔定义列表项。定义列表项的术语和分类器必须左对齐，定义必须相对于术语和分类器缩进

- **术语(term)** 是一个简单的单行单词或短语。转义前导连字符以防止识别为选项列表项。

- **可选的分类器(optional classifer)** 可以跟在同一行的术语之后，每个分类器在内联“ : ”（空格、冒号、空格）之后。在识别分类器定界符之前，会在术语行中解析内联标记。仅当分隔符出现在任何内联标记之外时才会被识别。

- **定义(definition)** 是相对于术语缩进的块，可以包含多个段落和其他正文元素。 **术语行和定义块之间不能有空行** （这将定义列表与块引用区分开来）。第一个定义列表项之前和最后一个定义列表项之后需要空行，但中间的空行是可选的。

例如::

    term 1
        Definition 1.

    term 2
        Definition 2, paragraph 1.

        Definition 2, paragraph 2.

    term 3 : classifier
        Definition 3.

    term 4 : classifier one : classifier two
        Definition 4.

    \-term 5
        Without escaping, this would be an option list item.

实际输出如下:

term 1
    Definition 1.

term 2
    Definition 2, paragraph 1.

    Definition 2, paragraph 2.

term 3 : classifier
    Definition 3.

term 4 : classifier one : classifier two
    Definition 4.

\-term 5
    Without escaping, this would be an option list item.

.. tip:: 定义列表可以以多种方式使用，包括：

    - 作为字典或术语表。术语是单词本身，可以使用分类器来指示术语的用法（名词、动词等），定义如下。

    - 描述程序变量。术语是变量名，分类器可用于指示变量的类型（字符串、整数等），定义描述变量在程序中的使用。定义列表的这种用法支持Grouch的分类器语法，这是一个用于描述和实施 Python 对象模式的系统。

字段列表(Field Lists)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Doctree element: field_list, field, field_name, field_body

字段列表由一个或多个字段(field)组成。每个字段由字段名称(field name)和字段主体(field body)组成。字段名称和字段主体之间的空行分隔字段。字段名称必须左对齐，字段主体必须相对于字段名称缩进。

字段名称是一个简单的单行单词或短语。字段主体是相对于字段名称缩进的块，可以包含多个段落和其他正文元素。字段名称和字段主体之间不能有空行（这将字段列表与块引用区分开来）。第一个字段列表项之前和最后一个字段列表项之后需要空行，但中间的空行是可选的。

字段名称以及单个冒号前缀和后缀一起构成字段标记。字段标记后面是 **空格** 和字段主体。字段主体可以包含多个相对于字段标记缩进的主体元素。字段名称标记后面的第一行确定字段主体的缩进。

字段列表的示例::

    :Date: 2001-08-16
    :Version: 1
    :Authors: - Me
              - Myself
              - I
    :Indentation: 由于字段标记可能很长，所以第二个
        并且字段主体的后续行不必与第一行对齐，
        但它们必须相对于第一行缩进
        字段名称标记，并且它们必须彼此对齐
    :Parameter i: integer

实际输出如下:

:Date: 2001-08-16
:Version: 1
:Authors: - Me
          - Myself
          - I
:Indentation: 由于字段标记可能很长，所以第二个
    并且字段主体的后续行不必与第一行对齐，
    但它们必须相对于第一行缩进
    字段名称标记，并且它们必须彼此对齐
:Parameter i: integer

选项列表(Option Lists)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Doctree element: option_list, option_list_item, option_group, option, option_string, description

选项列表由一个或多个选项列表项组成。每个选项列表项由一个或多个选项组成，后跟一个或多个描述。选项和描述之间的空行分隔选项列表项。选项列表项的选项必须左对齐，描述必须相对于选项缩进。

选项是一个或多个选项字符串(option string)的列表。选项字符串由一个或多个选项字符串元素(option string element)组成。选项字符串元素可以是一个或多个非空格字符，或者是一个或多个非空格字符，后跟一
个空格和一个可选的选项参数(option argument)。选项参数是一个或多个非空格字符。

描述是相对于选项缩进的块，可以包含多个段落和其他正文元素。选项和描述之间不能有空行（这将选项列表与块引用区分开来）。第一个选项列表项之前和最后一个选项列表项之后需要空行，但中间的空行是可选的。

选项列表的示例::

    -a         Output all.
    -c arg     Output just arg.
    --long     Output all day long.
    /V         A VMS/DOS-style option.

    -p         This option has two paragraphs in the description.
            This is the first.

            This is the second.
            Blank lines may be omitted between options
            (as above) or left in (as here and below).

    --very-long-option  A VMS-style option.  Note the adjustment
                        for the required two spaces.

    --an-even-longer-option
            The description can also start on the next line.

    -2, --two  This option has two variants.

    -f FILE, --file=FILE  These two options are synonyms; both have
                        arguments.

    -f <[path]file>  Option argument placeholders must start with
                    a letter or be wrapped in angle brackets.

    -d <src dest>    Angle brackets are also required if an option
                    expects more than one argument.

实际输出如下:

-a         Output all.
-c arg     Output just arg.
--long     Output all day long.
/V         A VMS/DOS-style option.

-p         This option has two paragraphs in the description.
           This is the first.

           This is the second.
           Blank lines may be omitted between options
           (as above) or left in (as here and below).

--very-long-option  A VMS-style option.  Note the adjustment
                    for the required two spaces.

--an-even-longer-option
           The description can also start on the next line.

-2, --two  This option has two variants.

-f FILE, --file=FILE  These two options are synonyms; both have
                      arguments.

-f <[path]file>  Option argument placeholders must start with
                 a letter or be wrapped in angle brackets.

-d <src dest>    Angle brackets are also required if an option
                 expects more than one argument.

文字块(Literal Blocks)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Doctree element: literal_block

由两个冒号（“::”）组成的段落表示以下文本块构成文字块。文字块必须缩进或加引号（见下文）。文字块内不包含任何标记处理。它保持原样，通常以等宽字体呈现：文字块会保留所有空白（包括换行符，但不包括缩进文字块的最小缩进）。文字块之前和之后都需要空行，但这些空行不包含在文字块中。

带缩进的文字块(Indented Literal Blocks)
"""""""""""""""""""""""""""""""""""""""

示例::

    This is a normal text paragraph.  The next paragraph is a code
    sample::

       It is not processed in any way, except
       that the indentation is removed.

       It can span multiple lines.

    This is a normal text paragraph again.

实际输出如下:

This is a normal text paragraph.  The next paragraph is a code
sample::

   It is not processed in any way, except
   that the indentation is removed.

   It can span multiple lines.

This is a normal text paragraph again.

带引号的文字块(Quoted Literal Blocks)
""""""""""""""""""""""""""""""""""""""""

以下都是有效的引用字符：

``！" # $ % & ' ( ) * + , - ./ : ; < = > ? @ [ \ ] ^ _ ` { | } ~``

示例::

    John Doe wrote::

    >> Great idea!
    >
    > Why didn't I think of that?

    You just did!  ;-)

实际输出如下:

John Doe wrote::

>> Great idea!
>
> Why didn't I think of that?

You just did!  ;-)

行块(Line Blocks)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Doctree element: line_block, line

行块是以竖线（“|”）前缀开头的行组。每个竖线前缀表示一个新行，因此保留换行符。初始缩进也很重要，导致嵌套结构。支持内联标记。续行是长行的包裹部分；它们以空格代替竖线开始。续行的左边缘必须缩进，但不必与其上方文本的左边缘对齐。行块以空行结束。

此示例说明了行块的嵌套，由新行的初始缩进表示::

    Take it away, Eric the Orchestra Leader!

        | A one, two, a one two three four
        |
        | Half a bee, philosophically,
        |     must, *ipso facto*, half not be.
        | But half the bee has got to be,
        |     *vis a vis* its entity.  D'you see?
        |
        | But can a bee be said to be
        |     or not to be an entire bee,
        |         when half the bee is not a bee,
        |             due to some ancient injury?
        |
        | Singing...

实际输出如下:

    Take it away, Eric the Orchestra Leader!

        | A one, two, a one two three four
        |
        | Half a bee, philosophically,
        |     must, *ipso facto*, half not be.
        | But half the bee has got to be,
        |     *vis a vis* its entity.  D'you see?
        |
        | But can a bee be said to be
        |     or not to be an entire bee,
        |         when half the bee is not a bee,
        |             due to some ancient injury?
        |
        | Singing...

.. tip::

    - 行块相当于可以换行的普通段落
    - 行块对于地址块、诗句（诗歌、歌词）和朴素的列表很有用。

块引用(Block Quotes)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Doctree element: block_quote, attribution

相对于前面的文本缩进且前面没有指示其是文字块或其他内容的标记的文本块是块引用。所有标记处理（对于正文元素和内联标记）在块引用内继续：

示例::

    This is an ordinary paragraph, introducing a block quote.

        "It is my business to know things.  That is my trade."

        -- Sherlock Holmes

实际输出如下：

This is an ordinary paragraph, introducing a block quote.

    "It is my business to know things.  That is my trade."

    -- Sherlock Holmes

.. tip:: 块引用就是带缩进的普通段落

文档测试块(Doctest Blocks)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Doctree element: doctest_block

Doctest 块是剪切并粘贴到文档字符串中的交互式 Python 会话。它们旨在通过示例说明用法，并通过Python 标准库中的doctest 模块提供优雅且强大的测试环境。

Doctest 块是以">>> "（Python 交互式解释器主提示符）开头，以空行结尾的文本块。Doctest 块被视为文字块的特殊情况，不需要文字块语法。如果两者都存在，则文字块语法优先于 Doctest 块语法：
::

    This is an ordinary paragraph.

    >>> print 'this is a Doctest block'
    this is a Doctest block

    The following is a literal block::

        >>> This is not recognized as a doctest block by
        reStructuredText.  It *will* be recognized by the doctest
        module, though!

实际输出如下:

This is an ordinary paragraph.

>>> print 'this is a Doctest block'
this is a Doctest block

The following is a literal block::

    >>> This is not recognized as a doctest block by
    reStructuredText.  It *will* be recognized by the doctest
    module, though!

.. tip:: Doctest 块一般用于Python 交互式解释器的会话

表格(Tables)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Doctree element: table, tgroup, colspec, thead, tbody, row, entry

ReStructuredText 提供了两种用于描述表格单元格的语法变体：网格表和简单表。表格也由CSV 表和List Table指令生成。table 指令用于添加表格标题或指定选项。

与其他正文元素一样，表格前后都需要空行。表格的左边缘应与前面的文本块的左边缘对齐；如果缩进，则该表被视为块引用的一部分。

一旦隔离，每个表格单元格都被视为一个微型文档；顶部和底部单元格边界充当分隔空白行的作用。每个单元包含零个或多个主体元素。单元格内容可能包括左边距和/或右边距，这些边距在处理之前被删除。

网格表(Grid Tables)
""""""""""""""""""""""""""

网格表通过类似网格的“ASCII 艺术”提供完整的表格表示。网格表允许任意单元格内容（主体元素）以及行和列跨度。然而，网格表的生成可能很麻烦，尤其是对于简单的数据集。请参阅简单表以获得更简单（但有限）的表示。

网格表用由字符“-”、“=”、“|”和“+”组成的可视网格来描述。连字符（“-”）用于水平线（行分隔符）。等号（“=”）可用于将可选标题行与表主体分开。竖线（“|”）用于垂直线（列分隔符）。加号（“+”）用于水平线和垂直线的交叉点。示例::

    +------------------------+------------+----------+----------+
    | Header row, column 1   | Header 2   | Header 3 | Header 4 |
    | (header rows optional) |            |          |          |
    +========================+============+==========+==========+
    | body row 1, column 1   | column 2   | column 3 | column 4 |
    +------------------------+------------+----------+----------+
    | body row 2             | Cells may span columns.          |
    +------------------------+------------+---------------------+
    | body row 3             | Cells may  | - Table cells       |
    +------------------------+ span rows. | - contain           |
    | body row 4             |            | - body elements.    |
    +------------------------+------------+---------------------+

实际输出如下:

+------------------------+------------+----------+----------+
| Header row, column 1   | Header 2   | Header 3 | Header 4 |
| (header rows optional) |            |          |          |
+========================+============+==========+==========+
| body row 1, column 1   | column 2   | column 3 | column 4 |
+------------------------+------------+----------+----------+
| body row 2             | Cells may span columns.          |
+------------------------+------------+---------------------+
| body row 3             | Cells may  | - Table cells       |
+------------------------+ span rows. | - contain           |
| body row 4             |            | - body elements.    |
+------------------------+------------+---------------------+

简单表格(Simple Table)
""""""""""""""""""""""""""""""""

简单表为简单数据集提供了紧凑且易于键入但有限的面向行的表表示形式。单元格内容通常是单个段落，尽管大多数单元格中可以表示任意正文元素。简单表允许多行行（除了第一列之外的所有行）和列跨度，但不允许跨行。有关完整的表格表示，请参阅上面的网格表。

简单的表格用由“=”和“-”字符组成的水平边框进行描述。等号（“=”）用于顶部和底部表格边框，并将可选标题行与表格主体分开。连字符（“-”）用于通过在连接的列下划线来指示单行中的列跨度，并且可以可选地用于显式地和/或视觉上分隔行。

简单的表格以等号的顶部边框开始，每列边界处有一个或多个空格（建议使用两个或多个空格）。无论跨度如何，顶部边框都必须完整描述所有表列。表中必须至少有两列（以区别于节标题）。顶部边框后面可能是标题行，最后一个可选标题行带有“=”下划线，列边界处也有空格。标题行分隔符下方不能有空行；它将被解释为表格的底部边框。表格的底部边界由“=”下划线组成，列边界处也有空格。例如，这是一个真值表，一个包含一个标题行和四个正文行的三列表格::

    =====  =====  =======
      A      B    A and B
    =====  =====  =======
    False  False  False
    True   False  False
    False  True   False
    True   True   True
    =====  =====  =======

实际输出如下:

=====  =====  =======
  A      B    A and B
=====  =====  =======
False  False  False
True   False  False
False  True   False
True   True   True
=====  =====  =======

下划线“-”可用于通过“填充”列边距以连接相邻列来指示列跨度。列跨度下划线必须完整（它们必须覆盖所有列）并与已建立的列边界对齐。包含列跨度下划线的文本行不得包含任何其他文本。列跨度下划线 **仅适用于紧邻其上方的一行** 。例如，这是一个表头中有一个列跨度的表::

    =====  =====  ======
       Inputs     Output
    ------------  ------
      A      B    A or B
    =====  =====  ======
    False  False  False
    True   False  True
    False  True   True
        True      True
    ============  ======

实际输出如下:

=====  =====  ======
   Inputs     Output
------------  ------
  A      B    A or B
=====  =====  ======
False  False  False
True   False  True
False  True   True
    True      True
============  ======

每行文本必须在列边界处包含空格，单元格通过列跨度连接的位置除外。每行文本都会开始一个新行，除非第一列中有空白单元格。在这种情况下，该行文本将被解析为续行。因此，新行（不是连续行）第一列中的单元格必须包含一些文本；空白单元格会导致误解（但请参阅下面的提示）。此外，此机制将第一列中的单元格限制为只有一行文本。如果此限制不可接受，请使用网格表。

以下示例说明了连续行（第 2 行由两行文本组成，第 3 行由四行文本组成）、分隔段落的空行（第 3 行、第 2 列）、延伸超过表格右边缘的文本以及新的文本。处理后输出的第一列中没有文本的行（第 4 行）::

    =====  =====
    col 1  col 2
    =====  =====
    1      Second column of row 1.
    2      Second column of row 2.
        Second line of paragraph.
    3      - Second column of row 3.

        - Second item in bullet
            list (row 3, column 2).
    \      Row 4; column 1 will be empty.
    =====  =====

实际输出如下:

=====  =====
col 1  col 2
=====  =====
1      Second column of row 1.
2      Second column of row 2.
       Second line of paragraph.
3      - Second column of row 3.

       - Second item in bullet
         list (row 3, column 2).
\      Row 4; column 1 will be empty.
=====  =====

其他
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. py:function:: enumerate(sequence[, start=0])

   Return an iterator that yields tuples of an index and an item of the
   *sequence*. (And so on.)
   thisis\ **one**\ word

由于 `缩进` *不够*，
该 ``段落`` 不属于列表
项（它是列表后面的块引用）。

------------

.. code:: shell

   pip install PyQt-Fluent-Widgets -i https://pypi.org/simple/

.. tabs::

   .. code-tab:: c

         C Main Function

   .. code-tab:: c++

         C++ Main Function

   .. code-tab:: py

         Python Main Function

   .. code-tab:: java

         Java Main Function

   .. code-tab:: julia

         Julia Main Function

   .. code-tab:: fortran

         Fortran Main Function

   .. code-tab:: r R

         R Main Function

.. tabs::

   .. code-tab:: c

         int main(const int argc, const char **argv) {
         return 0;
         }

   .. code-tab:: c++

         int main(const int argc, const char **argv) {
         return 0;
         }

   .. code-tab:: py

         def main():
            return

   .. code-tab:: java

         class Main {
            public static void main(String[] args) {
            }
         }

   .. code-tab:: julia

         function main()
         end

   .. code-tab:: fortran

         PROGRAM main
         END PROGRAM main

   .. code-tab:: r R

         main <- function() {
            return(0)
         }
