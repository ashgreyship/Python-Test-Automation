# evaluate

```text
# 三种参数是python表达式的关键字
1. should be true  用于断言
2. runkeyword if   用于条件判断
3. evaluate        用于利用python 快速赋值

*** Test Cases ***
case1
    ${even}  evaluate  [i for i in range(1,100) if i%2!=0]
    log to console  ${even}

case2
    ${var}  set variable  hello world
    ${res}  evaluate  $var[6:]
    log to console  ${res}

case3
    ${dict}  create dictionary  a=1  b=2
    log to console  ${dict}
    evaluate  $dict.update({"a":100})
    log to console  ${dict}

```

 

