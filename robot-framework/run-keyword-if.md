# run keyword if



```text
# 编程语言的几大要素
1. 变量--${scalar}  @{list}  &{dict}
2. 循环--FOR
3. 条件判断
4. 函数--用户关键词

*** Test Cases ***
条件判断
    ${time}  get time
    log to console  ${time}
    # 判断条件--python 条件表达式 待执行的关键字
    run keyword if  '2021:' in $time   Log to console  时间到，开始上课
    ...  ELSE IF  '08:' in $time  Log to console  现在是八月
    ...  ELSE  Log to console  不是2021

条件判断2
    FOR  ${one}  IN RANGE  99
        log to console  ${one} 分
        #   找出88
        run keyword if  $one==88  print and exit  # run keyword if 只能打印一个关键字
    END


条件判断3
    FOR  ${one}  IN RANGE  99
        run keyword if  $one==88  exit for loop  # 不会打印88
        log to console  ${one} 分
    END


*** Keywords ***
print and exit
    log to console  找到88分
    exit for loop

```

 

