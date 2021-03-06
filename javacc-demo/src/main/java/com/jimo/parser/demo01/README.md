# BraceDemo01

括号demo

编辑BraceDemo.jj, 然后使用 javacc编译。

```java
options {
    STATIC = false;
}

PARSER_BEGIN(BraceDemo)

package com.jimo.parser.demo01;

public class BraceDemo {

    public static void main(String[] args) throws Exception{
        final BraceDemo b = new BraceDemo(System.in);
        b.input();
    }
}
PARSER_END(BraceDemo)

void input():
{}
{
    MatchedBraces() ("\n" | "\r" | "\n\r")* <EOF>
}

void MatchedBraces():
{}
{
    "{" [ MatchedBraces() ] "}"
}
```

# BraceDemo02

再改改，支持空格：

```java
options {
    STATIC = true;
}

PARSER_BEGIN(BraceDemo)

package com.jimo.parser.demo01;

public class BraceDemo {

    public static void main(String[] args) throws Exception{
        final BraceDemo b = new BraceDemo(System.in);
        b.input();
    }
}
PARSER_END(BraceDemo)

SKIP : {
    " "
    | "\t"
    | "\n"
    | "\r"
}

void input():
{}
{
    MatchedBraces() <EOF>
}

void MatchedBraces():
{}
{
    "{" [ MatchedBraces() ] "}"
}
```

# BraceDemo03

计算括号个数
```java
options {
    STATIC = true;
    UNICODE_INPUT = true;
}

PARSER_BEGIN(BraceDemo)

package com.jimo.parser.demo01;

public class BraceDemo {

    public static void main(String[] args) throws Exception{
        final BraceDemo b = new BraceDemo(System.in);
        b.input();
    }
}
PARSER_END(BraceDemo)

SKIP : {
    " "
    | "\t"
    | "\n"
    | "\r"
}

TOKEN : {
    <LBRACE: "{">
    | <RBRACE: "}">
}

void input():
{int count;}
{
    count = MatchedBraces() <EOF>
    {
        System.out.println("level of brace :"+count);
    }
}

int MatchedBraces():
{int nested_count = 0;}
{
    <LBRACE> [nested_count = MatchedBraces() ] <RBRACE>
    {
        return ++nested_count;
    }
}
```
输出换行，Ctrl+D结束
```java
{{{}}}
^D
level of brace :3
```


