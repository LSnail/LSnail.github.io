---
date: 2015-09-12 00:14:31+00:00
layout: post
title: 简单的正则表达式
thread: 11
tag:
- TextView
feature: http://LSnail.github.io/assets/pictures/1317101.jpg
tags: iOS基础
---

# 1. 什么是正则表达式

正则表达式，又称正规表示法、常规表示法（英语：Regular Expression，在代码中常简写为regex、regexp或RE），计算机科学的一个概念。正则表达式使用单个字符串来描述、匹配一系列符合某个句法规则的字符串。在很多文本编辑器里，正则表达式通常被用来检索、替换那些符合某个模式的文本。

简单来说，当你需要检索一些比较有“特点”的字符串，或者想要对它做点什么，用普通的字符串处理方式又太过繁杂的时候，你就该考虑下使用正则表达式了。

# 2. 应该注意什么

别被那些复杂的表达式吓倒，只要从最简单的开始一步一步来，你会发现正则表达式其实并没有想像中的那么困难。当然，如果你真的耐下心来看完这篇文章了，能把提到过的语法记住80%以上的可能性为零。这里只是一些基本的原理，需要多多的联系。不过把它当做一个参考资料以便日后查阅，貌似也是一个不错的选择？
Let's get started!

学习正则表达式的最好方法是从例子开始，理解例子之后再自己对例子进行修改，实验。下面给出了不少简单的例子，并对它们作了说明。

假设你在一篇英文小说里查找hi，你可以使用正则表达式hi。

这几乎是最简单的正则表达式了，它可以精确匹配这样的字符串：由两个字符组成，前一个字符是h,后一个是i。通常，处理正则表达式的工具会提供一个忽略大小写的选项，如果选中了这个选项，它可以匹配hi,HI,Hi,hI这四种情况中的任意一种。

不幸的是，很多单词里包含hi这两个连续的字符，比如him,history,high等等。用hi来查找的话，这里边的hi也会被找出来。如果要精确地查找hi这个单词的话，我们应该使用\bhi\b。

\b是正则表达式规定的一个特殊代码（好吧，某些人叫它元字符，metacharacter），代表着单词的开头或结尾，也就是单词的分界处。虽然通常英文的单词是由空格，标点符号或者换行来分隔的，但是\b并不匹配这些单词分隔字符中的任何一个，它只匹配一个位置。

如果需要更精确的说法，\b匹配这样的位置：它的前一个字符和后一个字符不全是(一个是,一个不是或不存在)\w。

假如你要找的是hi后面不远处跟着一个Lucy，你应该用\bhi\b.*\bLucy\b。
这里，.是另一个元字符，匹配除了换行符以外的任意字符。*同样是元字符，不过它代表的不是字符，也不是位置，而是数量——它指定*前边的内容可以连续重复使用任意次以使整个表达式得到匹配。因此，.*连在一起就意味着任意数量的不包含换行的字符。现在\bhi\b.*\bLucy\b的意思就很明显了：先是一个单词hi,然后是任意个任意字符(但不能是换行)，最后是Lucy这个单词。

换行符就是'\n',ASCII编码为10(十六进制0x0A)的字符。

如果同时使用其它元字符，我们就能构造出功能更强大的正则表达式。比如下面这个例子：

0\d\d-\d\d\d\d\d\d\d\d匹配这样的字符串：以0开头，然后是两个数字，然后是一个连字号“-”，最后是8个数字(也就是中国的电话号码。当然，这个例子只能匹配区号为3位的情形)。

这里的\d是个新的元字符，匹配一位数字(0，或1，或2，或……)。-不是元字符，只匹配它本身——连字符(或者减号，或者中横线，或者随你怎么称呼它)。

为了避免那么多烦人的重复，我们也可以这样写这个表达式：0\d{2}-\d{8}。这里\d后面的{2}({8})的意思是前面\d必须连续重复匹配2次(8次)。


# 3. 常用的正则表达式

  1. ^/d+$　　//匹配非负整数 (正整数+0)
  2. ^[0-9]*[1-9][0-9]*$　　//匹配正整数
  3. ^((-/d+)|(0+))$　　//匹配非正整数（负整数 + 0）
  4. ^-[0-9]*[1-9][0-9]*$　　//匹配负整数
  5. ^-?/d+$　　　　//匹配整数
  6. ^/d+(/./d+)?$　　//匹配非负浮点数（正浮点数 + 0）
  7. ^(([0-9]+/.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*/.[0-9]+)|([0-9]*[1-9][0-9]*))$　　//匹配正浮点数
  8. ^((-/d+(/./d+)?)|(0+(/.0+)?))$　　//匹配非正浮点数（负浮点数 + 0）
  9. ^(-(([0-9]+/.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*/.[0-9]+)|([0-9]*[1-9][0-9]*)))$　　//匹配负浮点数
  10. ^(-?/d+)(/./d+)?$　　//匹配浮点数
  11. ^[A-Za-z]+$　　//匹配由26个英文字母组成的字符串
  12. ^[A-Z]+$　　//匹配由26个英文字母的大写组成的字符串
  13. ^[a-z]+$　　//匹配由26个英文字母的小写组成的字符串
  14.^[A-Za-z0-9]+$　　//匹配由数字和26个英文字母组成的字符串
  15. ^/w+$　　//匹配由数字、26个英文字母或者下划线组成的字符串
  16. ^[/w-]+(/.[/w-]+)*@[/w-]+(/.[/w-]+)+$　　　　//匹配email地址
  17. ^[a-zA-z]+://匹配(/w+(-/w+)*)(/.(/w+(-/w+)*))*(/?/S*)?$　　//匹配url
  18. 匹配中文字符的正则表达式： [/u4e00-/u9fa5]
  19. 匹配双字节字符(包括汉字在内)：[^/x00-/xff]
  20. 应用：计算字符串的长度（一个双字节字符长度计2，ASCII字符计1）String.prototype.len=function(){return this.replace([^/x00-/xff]/g,"aa").length;}
  21. 匹配空行的正则表达式：/n[/s| ]*/r
  22. 匹配HTML标记的正则表达式：/<(.*)>.*<///1>|<(.*) //>/
  23. 匹配首尾空格的正则表达式：(^/s*)|(/s*$)
  24. ^/S+[a-z A-Z]$ 不能为空 不能有空格  只能是英文字母
  25. /S{6,}         不能为空 六位以上
  26. ^/d+$          不能有空格 不能非数字
  27. (.*)(/.jpg|/.bmp)$ 只能是jpg和bmp格式
  28. ^/d{4}/-/d{1,2}-/d{1,2}$ 只能是2004-10-22格式
  29. ^0$            至少选一项
  30. ^0{2,}$        至少选两项
  31. ^[/s|/S]{20,}$ 不能为空 二十字以上
  32. ^/+?[a-z0-9](([-+.]|[_]+)?[a-z0-9]+)*@([a-z0-9]+(/.|/-))+[a-z]{2,6}$邮件
  33. /w+([-+.]/w+)*@/w+([-.]/w+)*/./w+([-.]/w+)*([,;]/s*/w+([-+.]/w+)*@/w+([-.]/w+)*/./w+([-.]/w+)*)* 输入多个地址用逗号或空格分隔邮件
  34. ^(/([0-9]+/))?[0-9]{7,8}$电话号码7位或8位或前面有区号例如（022）87341628
  35. ^[a-z A-Z 0-9 _]+@[a-z A-Z 0-9 _]+(/.[a-z A-Z 0-9 _]+)+(/,[a-z A-Z 0-9 _]+@[a-z A-Z 0-9 _]+(/.[a-z A-Z 0-9 _]+)+)*$;只能是字母、数字、下划线；必须有@和.同时格式要规范 邮件
  36. ^/w+@/w+(/./w+)+(/,/w+@/w+(/./w+)+)*$上面表达式也可以写成这样子，更精练
  37. ^/w+((-/w+)|(/./w+))*/@/w+((/.|-)/w+)*/./w+$
  38. final String CONDITION = "(?=.*[a-z])(?=.*[A-Z])(?=.*//d)";限定条件
  39. final String SPECIAL_CHAR = "[-A-Za-z0-9!$%&()/;<?{}//[//]^////]";允许出现的字符
  40. final String QUANTITY = "{8,16}";数量

# 4. 几个常用符号

(?=.*[a-z]) 表示当前位置后面必须出现 .*[a-z] 的字符，这个可以理解为必须出现小写字母。或者可以理解为某一个字符间的缝隙必须满足的条件，这个仅仅作为条件判断并不能匹配任何字符，因为这属于非捕获组中的环视（lookarround）零宽度匹配。

举个大家常见的例子：

表达式：Win(?=XP)
现有字符串 WinXP 和 WinNT，在应用这个表达式时前者能与之进行匹配，为什么呢？

当匹配器指示到 (?=XP) 时，也就是在 n 字母后面的缝隙，这个缝隙必须满足的条件是：后面的字符必须是 XP，如果是的话，匹配成功，否则匹配失败。由于(?=XP) 是匹配缝隙的，因此并不会把 XP 给匹配输出，而只输出了 Win 因此，这个表达式的语义可以看作是：找到后面为“XP”字符所有的 Win。

假如，我们把表达式写成 Win(?=XP)(?=NT) 这样的话，那么这个语义是：找出后面为“XP”并且为“NT”字符所有的 Win 可以想象，这是个永远无法满足的匹配。(?=XP)(?=NT) 这个表示当前的缝隙必须同时满足的条件。

把这个表达式再改一下，改成 Win(?=.*XP)(?=.*NT) 这个表示 Win 的后面必须出现XP 与 NT，位置和顺序都是无关的（这主要是 .* 的作用）。当然了这个表达式的效率是比较低的，得向后进行两次断言。

如果字符串是 WincbaXPabcNT 这个字符串，当匹配指示器走到 n 后面的缝隙时开始进行向后断言，首先对 .*XP 进行断言，很明显能将 cbaXP 匹配成功，这时第一个断言完成，再对 .*NT 断言，可以看出 cbaXPabcNT 能与其匹配成功，这时第二个断言完成，因此表达式 Win(?=.*XP)(?=.*NT) 能对 WincbaXPabcNT 进行匹配。

同理 WincbaNTabcXP 也是同样的效果。

如果能理解上面的这些，对于 (?=.*[a-z])(?=.*[A-Z])(?=.*//d) 这个的理应该不会很难吧，这个只不过是必须同时满足三个条件。

这个表达式在开始部分就进行断言，即索引为 0 的地方，也就是第一个字符的前面的缝隙，这个缝隙后面的字符必须满足 .*[a-z]  .*[A-Z]  .*//d  三个条件，也就是说必后面必须出现至少一个小写字母、至少一个大写母、至少一个数字。

^ 和 - 在 [  ] 结构的表达式中是有一定意义的。

[^abc] 表示除 abc 以外所有的字符，注意，这是放在最前面表示这个意思，如果改成 [a^bc] 这个仅表示 a ^ b c 四个字符。如果需要匹配 ^ 这个字符的话，千万不要把它放在第一个，如果一定要放在第一个的话，得使用转义符。

- 在 [  ] 表示字符的范围，比如 [a-z] 表示 a 与 z 之间的 26 个字母，[a-zA-Z] 这个表示 a-z 和 A-Z 的 52 个字母。使用范围得注意了，如果写成[z-a] 的话，在 Pattern.compile 编译表达式时会对范围进行检查，这时会产生异常，因此在使用 - 范围时，后面的 Unicode 值必须大于等于前面的 Unicode值。

如果要匹配“-”的话，尽量不要把 - 这个放在字符之间，可以放在 [  ] 的两边。比如 [-a-z] 这就能匹配 26 个小写字母和“-”了。当然了，我们也可以写成[a-z-A-Z] 这个可以匹配 52 字母和“-”，但是这样很不直观，我们宁愿写成[a-zA-Z-] 或者 [-a-zA-Z] 这样。

### 还有很多需要慢慢补充的，只有自己用到的时候才会真正理解某个表达式的意义。多用多想，其实没那么困难。
    