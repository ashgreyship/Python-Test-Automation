# collection



```text
*** Settings ***
Library  Collections

*** Test Cases ***
case1
    ${list}  create list
    log to console  ${list}
    FOR  ${one}  IN RANGE  100
    run keyword if  $one%2==0  append to list  ${list}  ${one}
    ...  ELSE  log to console  不是偶数
    END
    log to console  ${list}

case2
    ${list}  create list
    log to console  ${list}
    FOR  ${one}  IN RANGE  100
    append to list  ${list}  ${one}
    END
    remove from list  ${list}  1
    log to console  ${list}

case3
    ${dict}  create dictionary  a=1  b=2
    log to console  ${dict}
    set to dictionary  ${dict}  c=3
    log to console  ${dict}


```

 

