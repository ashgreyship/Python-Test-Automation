# 编码

## 常识

GBK编码兼容 ASCII 编码

UTF8 编码支持世界上所有语言（不是编码）

## 字符串和bytes转换

```python
a = b"abc"
print(type(a))

b = "中文字符串"
b = b.encode('utf8')  # 将字符串转换为 bytes 类型

print(type(b))
print(b)
print(b.decode('utf8'))  # 将 bytes 类型转换为字符串

```

## 获取编码类型

```python
import chardet

def get_encoding(file):
    with open(file, "rb") as f:
        data = f.read(1024)
        return chardet.detect(data)


encoding_data = get_encoding("./a.txt")
print(encoding_data)

#{'encoding': 'GB2312', 'confidence': 0.99, 'language': 'Chinese'}
```

## Unicode String 和 code 转换

```python
print(ord("字"))
# 输出一个char 的 Unicode code
print(chr(23383))
# 输出一个Unicode code 对应的 Unicode string
```

 

