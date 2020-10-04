## Q1:计算单词长度
```
题目描述：计算字符串最后一个单词的长度，单词以空格隔开。

输入描述：一行字符串，非空，长度小于5000。

输出描述：整数N，最后一个单词的长度。
```

以下为学习后的代码，说明：
1. str.strip()方法参数为空，默认的是空白符。
```
def get_len():
    input_str = input()
    if len(input_str) >= 5000 or len(input_str) == 0:
        print("重新输入")
        input_str = input()
    word = input_str.strip().split(' ')[-1]
    return len(word)

print(get_len())
```

## Q2:计算字符个数
```
题目描述：写出一个程序，接受一个由字母和数字组成的字符串，和一个字符，然后输出输入字符串中含有该字符的个数。不区分大小写。
输入描述:第一行输入一个有字母和数字以及空格组成的字符串，第二行输入一个字符。
输出描述:输出输入字符串中含有该字符的个数。
```

以下我写的代，说明：
1. 遍历计数
2. 对第二行字符没做判断
```
def f():
    first_row = input()
    second_row = input()
    count = 0
    for i in first_row:
        if i.upper() == second_row.upper():
            count+=1
    return count 
print(f())
```

优秀代码：
1. 对第二行字符做判断
2. 计算字符个数，使用split函数，拆分后的长度-1来计算，而非遍历
```
def ge_num():
    fir_line = input()
    sec_line = input()
    if len(sec_line) == 0 or len(sec_line) >1:
        return "第二行填入一个字符："
    leng = len(fir_line.strip().lower().split(sec_line.lower()))-1
    return leng
print(ge_num())
```

以下是优秀代码2，说明：
1. 使用a.count(b)来统计a中b出现的次数
```
def ge_num():
    fir_line = input()
    sec_line = input()
    if len(sec_line) == 0 or len(sec_line) >1:
        return "第二行填入一个字符："
    leng = fir_line.upper().count(sec_line.upper())
    return leng
print(ge_num())
```



### Q3 输入随机数
```
输入描述:
输入多行，先输入随机整数的个数，再输入相应个数的整数

输出描述:
返回多行，处理后的结果
```

以下是我的代码，几点说明：
1. 用户输入多组数据后，去重排序后一次输出
2. list.extend(obj)中的obj必须是可迭代对象，否则报错。int对象不是可迭代
3. list.sort()只适合列表方法，sorted(iterable)适合所有的可迭代对象
4. 该代码没通过测试用例，但逻辑无问题
5. 两个input()函数很有意思，值得反复品味
6. 使用try...except...中断while循环。当不输入即int(空)时，try捕获异常，进入except语句，跳出while循坏。

**`while try except 可以在无输入时结束程序`**
```
def rand_num():
    final_data = list()
    while True:
        try:
            num = int(input())
            for i in range(num):
                final_data.append(int(input('input int')))
        except:
            break
    target_data = sorted(set(final_data))
    for i in target_data:
        print(i)

rand_num()
```

以下为优秀代码，可以通过测试用例.区别在于：
1. 每次输入个数及数字后，及时输出当次去重排序结果
2. 使用集合set及set.add()方法
```
while True:
    try:

        a,res=int(input()),set()
        for i in range(a):res.add(int(input()))
        for i in sorted(res):print(i)


    except:
        break
```

## Q4: 字符串分隔
```
题目描述
•连续输入字符串，请按长度为8拆分每个字符串后输出到新的字符串数组；
•长度不是8整数倍的字符串请在后面补数字0，空字符串不处理。
输入描述:
连续输入字符串(输入2次,每个字符串长度小于100)

输出描述:
输出到长度为8的新字符串数组
```

我的代码，已通过测试用例：
1. 自定义函数，以字符串是否是8的倍数来做区分；当字符串不为8的倍数，添加若干个0至长度为8，过程中使用字符串索引和字符串拼接
2. 输入两次，调用两次函数即可
```
def get_str():
    ori_str = input()
    n = len(ori_str)
    if n%8 == 0:
        for ind in range(n//8):
            print(ori_str[8*ind:(8*ind+8)])
    else:
        for ind in range(n//8):
            print(ori_str[8*ind:(8*ind+8)])
        last_part_ind = n%8
        print(ori_str[(-1)*last_part_ind::1]+str(0)*(8-last_part_ind))
        
get_str()
get_str()
```


优秀代码：
1. 该代码特点在于：不区分字符串是否是8的倍数，而是根据实际情况酌情补0至字符串为8的倍数，值得学习
2. `line += "0" * (8 - left)`修改字符串

```
def chstr(line):
    left = len(line)%8
    if left != 0:
        line += "0" * (8 - left)
    for i in range(len(line) // 8):
        print(line[i*8:(i+1)*8])

a=input()
chstr(a)
b=input()
chstr(b)
```


## Q5:进制转换
```
题目描述
写出一个程序，接受一个十六进制的数，输出该数值的十进制表示。（多组同时输入 ）

输入描述:
输入一个十六进制的数值字符串。(0x开头)

输出描述:
输出该数值的十进制字符串。
```

经学习后，我写的代码如下:
1. 由于牛客网对输入输出要求很严格，因此不要类似print('exception')这种
2. 多行输入，每次输入都输出结果，因此，需要添加`while True: try： except：break`结构
3. 只要try语句有异常，就会执行except语句内容。本例中，字典get方法乘以int数值会存在报错当get方法返回None时
4. 对于可迭代序列，遍历索引，遍历值，两者完全不同
```
# while True:
#     try:
#         number = input()
#         n = len(number)
#         dic = {'0':0,'1':1,'2':2,'3':3,'4':4,'5':5,'6':6,'7':7,'8':8,'9':9,'A':10,'B':11,'C':12,'D':13,'E':14,'F':15}
#         final = 0
#         for i in range(2,n):
#             final += dic[number[i]]*(16**(n-i-1))
#         print(final)
#     except:
#         break

def hex_to_decimal():
    while True:
        try:
            hex_str = input()
        
            # 十六进制的A~F需要映射到数字
            hex_dic = {'0':0,'1':1,'2':2,'3':3,'4':4,'5':5,'6':6,'7':7,'8':8,'9':9,'A':10,
                       'B':11,'C':12,'D':13,'E':14,'F':15}
            length = len(hex_str)
            dec_num = 0
            # 逐位累加
            for index in range(2,length):
                # 使用字符串索引，
                dec_num += hex_dic.get(hex_str[index])*(16**(length-1-index))
            print(dec_num)
        except:
#             print('exception')
            break

if __name__ == '__main__':
    hex_to_decimal()
```

5. 十六进制值需要映射，以下代码提供了另一种思路，倒序遍历十六进制数
```
s=input()
dic_map={'A':10,'B':11,'C':12,'D':13,'E':14,'F':15}
count=0
k=0
for i in s[:1:-1]:
    if i.isdigit()==True:
        count+=int(i)*(16**k)
        k+=1
    else:
        count+=dic_map[i]*(16**k)
        k+=1
print(count)
```


## Q6：取近似值
```
题目描述
写出一个程序，接受一个正浮点数值，输出该数值的近似整数值。如果小数点后数值大于等于5,向上取整；小于5，则向下取整。

输入描述:
输入一个正浮点数值

输出描述:
输出该数值的近似整数值
```

以下我的代码：
1. **注意代码第三行，判断是否大于五，字符串要转化为int才能比较**
2. 代码第二行，查找小数点字符，当输入字符串无小数点时，返回-1。
3. 代码考虑的其他情况较少，只限通过测试用例，有优化空间。
```python
numstr = input()
ind=numstr.find('.')
if int(numstr[ind+1])>=5:
    print(int(numstr[0:ind])+1)
else:
    print(int(numstr[0:ind])) 
```

优秀代码1：
1. 将输入字符串转化为float，加0.5取整。举例，3.4+0.5=3.9，取整是多少？还得了解int()函数
2. `int 接收浮点数作为参数，会截取该浮点数的整数部分，返回截取后的整数。` 只是截断浮点数的整数部分，截断。
3. int(x,base=10)还能做进制转换
```
print(int(float(input())+0.5))
```

优秀代码2：
1. 由于python对于浮点数存储有点抽风（4.5会存储成4.4999999），所以要加上0.001
2. round( x [, n]  )函数返回浮点数的 四舍五入值。
3. 关于浮点数的存储，有更深的拓展知识。
```
print(round(float(input())+0.001))
```

## Q7：质数因子
```
题目描述
功能:输入一个正整数，按照从小到大的顺序输出它的所有质因子（重复的也要列举）（如180的质因子为2 2 3 3 5 ）

最后一个数后面也要有空格

输入描述:
输入一个long型整数

输出描述:
按照从小到大的顺序输出它的所有质数的因子，以空格隔开。最后一个数后面也要有空格。
```

学习到的代码：
1. 该代码思路：
  - 输入正整数，质因子从2开始累加迭代
  - 若输入数能整除质因子，则将质因子添加到列表保存，并迭代输入数为整除的结果
    - 整除代码需要while判断，若能被一个质因子多次整除，需要重复添加到列表，因为题目要求重复也要列举
  - 若输入数不能整除质因子，则质因子累加
  - 输出符合要求格式
2. 质因子一定从2开始，最多到输入数一半
  - 当输入数为质数时，要单独输出结果
3. `" ".join(map(str, res)) + " " if res else str(a) + " "`  这种代码还是很不习惯，得多熟练

```python
a, res = int(input()), []
for i in range(2, a // 2 + 1):
    while a % i == 0:
        a = a / i
        res.append(i)
print(" ".join(map(str, res)) + " " if res else str(a) + " ")
```

## Q8：合并表记录
```
题目描述
数据表记录包含表索引和数值（int范围的整数），请对表索引相同的记录进行合并，即将相同索引的数值进行求和运算，输出按照key值升序进行输出。

输入描述:
先输入键值对的个数
然后输入成对的index和value值，以空格隔开

输出描述:
输出合并后的键值对（多行）
```

我的代码：
1. 相同键的值相加与字典计数一样，区别在于本例是+value 而非+1
2. sorted(iterable)
3. 输入的数字型字符，若实现加法需要转化为int
```python
def dic_comb():
    n = int(input())
    dic = dict()
    for i in range(n):
        string = input()
        key, value = int(string.split(' ')[0]), int(string.split(' ')[1])
        # dic[key] += dic.get(key, value)
        # 计算加法时，不能使用+=，因为初始dic[key]会报错。
        # dic[key] = dic.get(key, value)+value，语法正确，但加出来的值完全不对
        dic[key] = dic.get(key,0)+value  
    # 输出合并后的键值对
    for k in sorted(dic.keys()):
        print(k, dic[k])
```



## Q9：提取不重复整数
```
题目描述
输入一个int型整数，按照从右向左的阅读顺序，返回一个不含重复数字的新的整数。

输入描述:
输入一个int型整数

输出描述:
按照从右向左的阅读顺序，返回一个不含重复数字的新的整数
```

我的代码：
1. 起初预想是使用set去重，实操后很麻烦
2. 一边倒序遍历，一边将不重复字符添加到结果变量res中。预想使用空字符''替代，实操发现不需要。

```python
n = input()
res = ''
for i in n[::-1]:
    if i in res:
        # 若已存在，pass，不保存在res中
        pass
    else:
        # 都是字符，+=拼接
        res += i
print(res)
```




