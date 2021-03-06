# Yaml

## 注意事项

* 大小写敏感
* 使用缩进表示层级关系
* 缩进时不允许使用Tab，只允许使用空格
* 缩进的空格数目不重要，只要相同层级的元素左对齐即可
* yaml中有字符串丶整型丶浮点型丶布尔型丶时间类型。
* 用于引用 变量
* yaml是可以进行强制转换的，用 !! 实现
* 在同一个yaml文件中，可以用 --- 来分段，这样可以将多个文档写在一个文件中

## 格式

### 字典类型

```yaml
name: tom
age: 20
```

###  列表

```yaml
- 10
- 20
- 30
```

### 复合 

```yaml
- info :
    - 10
    - 20
    - 30
- info2 :
    - 2
```

###  模式选择

```yaml
mode : on
mode2 : off
```

```python
{'mode': True, 'mode2': False} # 自动转换为 true 和 false
```

### 定义变量

```yaml
name: &var myname   # 定义变量
data: *var    # 使用变量
```

### 分段

```yaml
companies:
  - id: 1
    name: company1
    price: 200W
  - id: 2
    name: company2
    price: 500W
---
companies2:
  - id: 1
  - name: company1
  - price: 200W

  - id: 2
  - name: company2
  - price: 500W
```

## Yaml 转换成字典

```python
import yaml

yamlDir = './config/test.yaml'

fo = open(yamlDir, 'r', encoding='utf-8')

res = yaml.load(fo, Loader=yaml.FullLoader)
print(res)
print(res["name"])
```

### 读取多段yaml文件

```python
import pprint

import yaml


def get_yaml_data():
    yaml_dir = '../config/test.yaml'

    fo = open(yaml_dir, 'r', encoding='utf-8')

    # res = yaml.load(fo, Loader=yaml.FullLoader)
    res = yaml.load_all(fo, Loader=yaml.FullLoader)
    for one in res:
        pp = pprint.PrettyPrinter()
        pp.pprint(one)


get_yaml_data()
```

 

## 字典转换为 Yaml

```python
yamlDir = './config/test2.yaml'
fo = open(yamlDir, 'w', encoding='utf-8')
runData = {'info': 'normal', 'time': '2002'}
res = yaml.dump(runData, fo)
```

 



