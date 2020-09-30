## Q1:计算单词长度
题目描述
计算字符串最后一个单词的长度，单词以空格隔开。
输入描述:
一行字符串，非空，长度小于5000。

输出描述:
整数N，最后一个单词的长度。

```
def get_len():
    input_str = input()
    word = input_str.strip().split(' ')[-1]
    if len(word)>5000 or len(word) == 0:
        return "重新输入"
    return len(word)

print(get_len())
```

## Q2:

