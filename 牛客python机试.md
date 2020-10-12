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

4. 2020年10月10日21:03:59我再次手动写的代码，并调试通过，有几点感受：
- 为什么要设置变量res？是为了根据res值来判断输入的n是否是质数
- 为什么res要设置为列表？因为列表可以添加，且列表可以转换为字符串，str.join()即可
- 为什么要map(str,res)？因为res的元素都是整数，需要map为str类型，与空格字符串拼接。数字" ".join([2,3,4])会报错，`TypeError: sequence item 0: expected str instance, int found`
- i遍历，后续的n%i n=n/i都是用i，而不是用数字2
- 调试代码才能发现测试用例哪些不通过

```python
n = int(input())
res = []
for i in range(2,n//2+1):
    while n%i == 0:
        n = n/i
        res.append(i)
#         print("%s "%i)
if not res:
    print(str(n)+' ')
else:
    print(" ".join(map(str,res))+" ")
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

## Q10：
```
题目描述
将一个英文语句以单词为单位逆序排放。例如“I am a boy”，逆序排放后为“boy a am I”
所有单词之间用一个空格隔开，语句中除了英文字母外，不再包含其他字符

输入描述:
将一个英文语句以单词为单位逆序排放。

输出描述:
得到逆序的句子
```

我的代码，由于想一行代码解决，使用list.reverse()，结果并不了解reverse()方法，导致错误代码
```python
# 错误代码1：list.reverse()并不会返回新的列表
# temp = input().split(' ').reverse()
# print(' '.join(temp))

# 错误代码2：list.reverse()并不会返回
# temp = input().split(' ')
# print(' '.join(temp.reverse()))

temp = input().split(' ')
temp.reverse()  # 直接修改temp列表
print(' '.join(temp))
```

若不使用reverse()呢?对于列表a，a[::1] 或a[:] 或a[::] 都是正序切片取所有元素，a[::-1]逆序取所有元素
```python
print(' '.join(input().split(' ')[::-1]))
```

## Q11:
```
题目描述
给定n个字符串，请对n个字符串按照字典序排列。
输入描述:
输入第一行为一个正整数n(1≤n≤1000),下面n行为n个字符串(字符串长度≤100),字符串中只含有大小写字母。
输出描述:
数据输出n行，输出结果为按照字典序排列的字符串。
```

我的代码
1. 最初的思路是，将输入的字符串计算所有字符ASCII码和，赋值为weight，按和排序这些字符串。但写到一半，发现问题：'ab'和'ba'的weight一样，这种思路排序失败
2. sorted()直接实现排序，按行输出即可
```python
# n = int(input())
# dic = dict()
# for i in range(n):
#     str_i = input()
#     str_i = str_i.lower()
#     weight = 0
#     for char in str_i:
#         weight += ord(char)
#     dic[str_i] = weight
# # 按weight排序dic
# for key,value in dic.items():
    
n = int(input())
str_list = [input() for i in range(n)]
for i in sorted(str_list):
    print(i)
```
3. 虽然最初计算weight的思路不行，但如何完成根据值value来排序key呢？
```python
# 思路1：将weight设为key，只限于key不相同
b = {65:'b',23:'ac',98:'d'}
print(sorted(b))
for i in sorted(b):
    print(b[i])

# 思路2：将weight设为value
a = {'b':65,'ac':23,'d':98}
value_list = [value for value in a.values()]
value_list.sort()
def get_key(dic,value):
    for k,v in dic.items():
        if v == value:
            return k
    # return [k for k,v in dic.items() if v==value]
for i in value_list:
    # 如何从字典a中根据值i找出键key
    temp =get_key(a,i)
    print(temp)
```
4. 思路2的另一种代码。
   - `list(a.values()).index(i)`是列表value，找出特别value对应的列表索引，简记为ind
   - `list(a.keys())[ind]`是列表key，取出索引ind的key
   - 列表化，从key、value两个列表并进，利用key-value对索引一样来操作，很是精巧。使用list.index()方法，
```python
a = {'b':65,'ac':23,'d':98}
value_list = [value for value in a.values()]
value_list.sort()
for i in value_list:
    print(list(a.keys())[list(a.values()).index(i)])
```

5. 使用字典生成式由value查找key。**若出现value相同的情况，该方法就不行。如a = {'b':65,'ac':65,'d':98},temp是{65: 'ac', 98: 'd'}**
```python 
a = {'b':65,'ac':23,'d':98}
value_list = [value for value in a.values()]
value_list.sort()
# 互换k,v的字典
temp = {v:k for k,v in a.items()}
print(temp)
for i in value_list:
    print(temp[i])
```

## Q12：二进制1的个数
```
题目描述
输入一个int型的正整数，计算出该int型数据在内存中存储时1的个数。

输入描述:
 输入一个整数（int类型）

输出描述:
 这个数转换成2进制后，输出1的个数
 ```
 
 我的代码：
 1. 对于str，也可以直接使用.count('1')
 2. bin('3')，返回'0b11'，是否取[2:]对1的计数没影响
 3. 讨论区看到一个计数思路，二进制将0全部替换为空字符串，计算替代后的字符串的长度
 ```python
 n = int(input())
charlist = list(bin(n)[2:])
print(charlist.count('1'))
 ```
 
 ## Q13：坐标移动
 ```
 题目描述
开发一个坐标计算工具， A表示向左移动，D表示向右移动，W表示向上移动，S表示向下移动。从（0,0）点开始移动，从输入字符串里面读取一些坐标，并将最终输入结果输出到输出文件里面。

输入：

合法坐标为A(或者D或者W或者S) + 数字（两位以内）

坐标之间以;分隔。

非法坐标点需要进行丢弃。如AA10;  A1A;  $%$;  YAD; 等。

下面是一个简单的例子 如：

A10;S20;W10;D30;X;A1A;B10A11;;A10;

处理过程：

起点（0,0）

+   A10   =  （-10,0）

+   S20   =  (-10,-20)

+   W10  =  (-10,-10)

+   D30  =  (20,-10)

+   x    =  无效

+   A1A   =  无效

+   B10A11   =  无效

+  一个空 不影响

+   A10  =  (10,-10)

结果 （10， -10）

注意请处理多组输入输出

输入描述:
一行字符串

输出描述:
最终坐标，以,分隔
```
 
 
 
 错误代码，错误记录：
 1. 当输入不合法坐标时，不能使用coor.remove(i)，造成的结果是`在下一步遍历时会漏掉列表元素`，因此新增valid_coor来保存合法坐标。
 2. 当某个坐标为'A1A'时，int(i[1:])就会报错。该代码的目的是：判断后两位字符是数字，1~99。
 ```python
 def position_cal():
    while 1:
        try:
            coor = input().split(';')
            valid_coor = []
            for i in coor:
                if i == '':
                    pass
                elif len(i) >= 2 and len(i) <= 3 and i.startswith(
                        ('A', 'W', 'S', 'D')) and int(i[1:]) >= 1 and int(
                        i[1:]) <= 99:
                    valid_coor.append(i)
                else:
                    pass
            ad = 0  # x坐标值
            ws = 0  # y坐标值
            for j in valid_coor:
                if j == '':
                    pass
                elif j[0] == 'A':
                    ad -= int(j[1:])
                elif j[0] == 'D':
                    ad += int(j[1:])
                elif j[0] == 'W':
                    ws += int(j[1:])
                elif j[0] == 'S':
                    ws -= int(j[1:])
                else:
                    pass
            print(ad, ws)
        except:
            break


position_cal()
```
3. 通过测试用例的代码，注意点有：
- **若想确定字符串是两位数字，使用str.isdigit()是个好办法，同时限定字符串长度**；此前的将字符串转化为int，int值在1~99，会有很多问题
- 注意题目输出，`print(str(ad)+','+str(ws))`
```python
def position_cal():
    while 1:
        try:
            coor = input().split(';')
            valid_coor = []
            for i in coor:
                if i == '':
                    pass
                elif len(i) >= 2 and len(i) <= 3 and i.startswith(
                        ('A', 'W', 'S', 'D')) and i[1:].isdigit():
                    valid_coor.append(i)
                else:
                    pass
            ad = 0  # x坐标值
            ws = 0  # y坐标值
            for j in valid_coor:
                if j == '':
                    pass
                elif j[0] == 'A':
                    ad -= int(j[1:])
                elif j[0] == 'D':
                    ad += int(j[1:])
                elif j[0] == 'W':
                    ws += int(j[1:])
                elif j[0] == 'S':
                    ws -= int(j[1:])
                else:
                    pass
            print(str(ad)+','+str(ws))
        except:
            break


position_cal()
```

4. 讨论区优秀代码，优秀之处在于：
- 遍历一次坐标列表
- str.isdigit()只有全为数字且不为空字符串才返回True，''.isdigit()返回False。因此`i[1:].isdigit()`若为True则表明至少有一个数字
- 输出特定格式，使用str.format()方法，也可使用print("%s,%s"%(cd[0],cd[1]))
- 使用一个列表保存x和y的坐标值，看起来比两个变量保存要简洁
```python
while 1:
    try:
        s_tr = input()
        cd = [0,0]
        li = s_tr.split(";") # 字符串分割存入列表
        for i in li:
            if i.startswith("A") and len(i) <= 3 and i[1:].isdigit():
                cd[0] += -int(i[1:])
            elif i.startswith("D") and len(i) <= 3 and i[1:].isdigit():
                cd[0] += int(i[1:])
            elif i.startswith("W") and len(i) <= 3 and i[1:].isdigit():
                cd[1] += int(i[1:])
            elif i.startswith("S") and len(i) <= 3 and i[1:].isdigit():
                cd[1] += -int(i[1:])
            else:
                continue
        print("{0},{1}".format(cd[0], cd[1]))
    except:
        break
```
5. 另一优秀代码，虽然不能连续输入，但思想值得学习.
- 在遍历每个坐标时，先判断首个字符是不是特定字母，再接着判断后续字符是不是数字，使用了try...except:pass。当try因字符串非数字而出现异常时，进入except语句pass，最终的结果是：非法坐标不影响代码中断，也不影响最后输出结果
- x和y的计算也特别有意思，非常简洁而抽象。由于x,y都得执行，因此设置dx，dy表示方向，保证相同索引只有一个非零。
```python
import sys
dx = [-1, 0, 0, 1]
dy = [0, -1, 1, 0]
for line in sys.stdin:
    x, y = 0, 0
    for cmd in line.split(';'):
        cmd = cmd.strip()
        if cmd and cmd[0] in 'ASWD':
            try:
                n = int(cmd[1:])
                x += n * dx['ASWD'.find(cmd[0])]
                y += n * dy['ASWD'.find(cmd[0])]
                #print cmd, n, x, y
            except:
                pass
    print("%d,%d"% (x, y))
```

## Q14：背包问题



## Q15：最小公倍数
我的代码，运行时间过长，因为遍历到mxn
```python
a,b = map(int,input().split())
while 1:
    try:
        for i in range(max(a,b),a*b+1):
            if i%a==0 and i%b==0:
                print(i)
                break
    except:
        break
```

优秀代码，利用最大公约数与最小公倍数乘积等于两整数积求解
```python
def gcd(a, b):
    """Return greatest common divisor using Euclid's Algorithm."""
    while b:
        a, b = b, a % b
    return a
def lcm(a, b):
    """Return lowest common multiple."""
    return a * b // gcd(a, b)
    
while True:
    try:
        a,b=map(int,input().split())
        print(lcm(a,b))
    except:
        break
```

## Q16：
```
题目描述
从输入任意个整型数，统计其中的负数个数并求所有非负数的平均值，结果保留一位小数，如果没有非负数，则平均值为0
本题有多组输入数据，输入到文件末尾，请使用while(cin>>)读入
数据范围小于1e6
输入描述:
输入任意个整数

输出描述:
输出负数个数以及所有非负数的平均值
```

我的代码
```python
data = []
while 1:
    try:
        n = int(input())
        data.append(n)
    except:
        break
        
neg = 0
pos = []
for i in data:
    if i<0:
        neg+=1
    else:
        pos.append(i)
print(neg)
sum,count=[0,0]
for j in pos:
    count+=1
    sum+=j
print(round(sum/count,1))        
```

除了输入数据外，后续统计负数个数以及非负数平均值，代码冗余。以下是精简代码：
- 主要在于使用if语句，在完整的语句后加if...else...
```python

data = []
while 1:
    try:
        n = int(input())
        data.append(n)
    except:
        break
a, pos, neg = data, [], []
for i in a:
    neg.append(int(i)) if int(i) < 0 else pos.append(int(i))
print(len(neg))
print(round(sum(pos) / len(pos), 1) if pos else "0.0")
```

## Q17：求解立方根
```
题目描述
•计算一个数字的立方根，不使用库函数

详细描述：

•接口说明

原型：

public static double getCubeRoot(double input)

输入:double 待求解参数

返回值:double  输入参数的立方根，保留一位小数


输入描述:
待求解参数 double类型

输出描述:
输入参数的立方根 也是double类型
```

其实该道题转化为数学问题，在数学上如何实现立方根求解，说明：
1. 学习完牛顿迭代法，求解立方根x等于按照迭代公式迭代x
2. 代码逻辑：求n的立方根，初始化x=1，按照牛顿迭代法公式迭代x直至 x接近立方根(x三次方与n的绝对值小于1e-7)，输出。
```python
n = float(input())
x = 1
while abs(x**3-n)>1e-7:
    x = (2*x/3)+n/3/x/x
print(round(x,1))
```

## Q18：字符次数统计
```
题目描述
如果统计的个数相同，则按照ASCII码由小到大排序输出 。如果有其他字符，则对这些字符不用进行统计。

输入描述:
输入一串字符。

输出描述:
对字符中的
各个英文字符（大小写分开统计），数字，空格进行统计，并按照统计个数由多到少输出,如果统计的个数相同，则按照ASII码由小到大排序输出 。如果有其他字符，则对这些字符不用进行统计。
```

我的代码，相关说明：
1. 使用固定结构`while 1 try code except break`保证持续输入
2. 代码没有对英文字符外的做限制，若做限制，需要使用遍历字符串，每个字符使用多条件or来判断， str.isalpha() str.isdigit() str == ' '
3. 使用字典统计输入字符串各字符次数
4. 列表value_list统计字典值次数，去重
5. list.sort()默认升序，默认reverse=False；升序需要reverse=True

```python
while 1:
    try:
        st = input()
        # dic计数
        dic = dict()
        for i in st:
            dic[i] = dic.get(i,0)+1
        # 对dic值，去重后降序
        value_list = list(set([j for j in dic.values()]))
        value_list.sort(reverse=True)
        res = [ ]
        for m in value_list:
            # 若次数相同，对多个key做升序，即ASCII码由小到大
            temp = [k for k,v in dic.items() if v==m] 
            temp.sort(reverse=False)
            # 使用extend而非append
            res.extend(temp)
        print(''.join(res))
    except:
        break
```

评论区代码，粘贴一个，可以通过测试用例，说明：
1. 使用set()和字典来统计字符串次数
2. 对

```python
while True:
    try:
        from collections import defaultdict

        dd, s, res = defaultdict(list), input(), ""
        for i in set(s):
            dd[s.count(i)].append(i)

        for i in sorted(dd.keys(), reverse=True):
            res += "".join(sorted(dd[i], key=ord))
        print(res)
    except:
        break

```

## Q19:
```
题目描述
输入整型数组和排序标识，对其元素按照升序或降序进行排序（一组测试用例可能会有多组数据）

输入描述:
输入需要输入的整型数个数，排序标识：0表示按升序，1表示按降序

输出描述:
输出排好序的数字

输入：
8
1 2 4 9 3 55 64 25
0

输出：
1 2 3 4 9 25 55 64
```

我的代码，几点说明：
1. 输入的n个整数是一行字符串的形式，不需要遍历range(n)。**注意区分[多个换行输入](https://www.nowcoder.com/practice/3245215fffb84b7b81285493eae92ff0?tpId=37&&tqId=21226&rp=1&ru=/ta/huawei&qru=/ta/huawei/question-ranking)**
2. `input_list = list(map(int,input_str.split()))`需要**将map()对象转化为list类型**，后续才能排序。
    ```
    >>> map(str,[1,2,5])
    <map object at 0x000001E43D92B408>
    >>> "+".join(map(str,[1,2,5]))
    '1+2+5'
    ```
3. 由于输出是一行，使用print()的end=' '，需要额外添加换行才能通过测试用例
4. 在输出这一块，使用`print(" ".join(map(str,input_list)))`可代替代码输出的三行，区别在于：代码中的输出的是数字，第四点提到的是输出字符串且一行代码足矣
5. 字符大小与整数大小比较不同，注意转换。**如'5>'11',5<11**
```python
while 1:
    try:
        n = int(input())
        input_str = input()
        flag = int(input())
        dic = {0:False,1:True}
        input_list = list(map(int,input_str.split()))
        input_list.sort(reverse=dic[flag])
        for i in input_list:
            print(i,end=' ')
        print()  
    except:
        break
```

## Q20：自守数
```
题目描述
自守数是指一个数的平方的尾数等于该数自身的自然数。例如：25^2 = 625，76^2 = 5776，9376^2 = 87909376。请求出n以内的自守数的个数

输入描述:
int型整数

输出描述:
n以内自守数的数量。
```

我的代码如下，几点说明：
1. 核心思想，计算i*i 与 i，将其转化为字符串，有'625'.endswith('25')返回True
2. 通过测试用例，发现0也算自守数，因此`for i in range(0,n+1)`从0开始共n+1个数
```python
while 1:
    try:
        n = int(input())
        count = 0
        for i in range(1,n+1):
            i2 = i*i 
            if str(i2).endswith(str(i)):
                count += 1
        print(count)
    except:
        break
```

## Q21: 投票统计
```
输入描述:
输入候选人的人数，第二行输入候选人的名字，第三行输入投票人的人数，第四行输入投票。

输出描述:
每行输出候选人的名字和得票数量。

输入：
4
A B C D
8
A B C D E F G H
输出：
A : 1
B : 1
C : 1
D : 1
Invalid : 4
```

以下我的代码，没通过测试用例，**在于输出格式不对，没有按照候选人顺序输出票数**，代码要点说明：
1. 原以为输出按照候选人字母升序，因此单独将统计字典的key以升序列表保存。后续按照列表遍历，输出，但测试用例不通过，因为不是按候选人顺序输出
2. 字典dic，`sorted(dic)`只返回升序key会丢失value；**字典生成式是否会按顺序生成新字典还有待商榷**
    ```
    >>> dic={'F':12,'C':2,'D':4,'A':7}
    >>> asc=sorted(dic)
    >>> asc
    ['A', 'C', 'D', 'F']
    >>> {i:dic[i] for i in asc}
    {'A': 7, 'C': 2, 'D': 4, 'F': 12}
    ```
3. 对于输出invalid部分，采用`if i in c_name:`做判断。这样做是因为计算invalid没想到好办法，进而导致输出不是候选人顺序。
```python
while 1:
    try:
        c,c_name,v,v_ticket = int(input()),input().split(),int(input()),input().split()
        dic = {}
        # 统计票数
        for k in v_ticket:
            dic[k] = dic.get(k,0)+1
        # 将字典按key升序排列，以遍历时按顺序输出
        key = sorted(list(dic.keys()))
        # dic2 = {i:dic[i] for i in key }

        # 输出，按格式
        invalid = 0
        for i in key:
            if i in c_name:
#                 print("%s : %s"%(i,dic[i]))
                print(i+" : "+str(dic[i]))
            else:
                invalid += 1
#         print("Invalid : %s"%invalid)
        print("Invalid : "+str(invalid))
    except:
        break
```
4. 修改后的且通过测试用例的代码，与上述代码区别在于：
- 上述代码，遍历投票人，若投票人不在候选人名单中，invalid += 1
- 修改后代码，遍历候选人名单，同时统计有效票数valid，**使用总投票数减去有效票数得到无效票数，很巧妙**
- 为了通过测试用例，**不放弃不敷衍，终于通过测试用例，不通过不会是偶然的，还是有原因的**
    ```
    4
    A B C D
    8
    E F G H E F G H
    ```
- 使用字典计数是没问题的，关键在于后面的遍历候选人，dic[i]会由于i不存在而抛出异常进入except语句
- 若使用collections模块的Counter计数，如`dic = Counter(v_ticket)`，即使访问不存在的key时也会返回0，访问方式只有`dic[i]`
- 这样的深究很有价值啊
```python
# from collections import Counter
while 1:
    try:
        c,c_name,v,v_ticket = int(input()),input().split(),int(input()),input().split()
        dic = {}
        # 统计票数
        for k in v_ticket:
            dic[k] = dic.get(k,0)+1
#         dic = Counter(v_ticket)

        # 输出，按候选人顺序格式
        valid = 0
        for i in c_name:
            print("%s : %s"%(i,dic.get(i,0)))
            valid += dic.get(i,0) 
        print("Invalid : %s"%(len(v_ticket)-valid))
    except:
        break
```

## Q22: 空瓶子喝汽水问题
```
题目描述
有这样一道智力题：“某商店规定：三个空汽水瓶可以换一瓶汽水。小张手上有十个空汽水瓶，她最多可以换多少瓶汽水喝？”答案是5瓶，方法如下：先用9个空瓶子换3瓶汽水，喝掉3瓶满的，喝完以后4个空瓶子，用3个再换一瓶，喝掉这瓶满的，这时候剩2个空瓶子。然后你让老板先借给你一瓶汽水，喝掉这瓶满的，喝完以后用3个空瓶子换一瓶满的还给老板。如果小张手上有n个空汽水瓶，最多可以换多少瓶汽水喝？
输入描述:
输入文件最多包含10组测试数据，每个数据占一行，仅包含一个正整数n（1<=n<=100），表示小张手上的空汽水瓶数。n=0表示输入结束，你的程序不应当处理这一行。

输出描述:
对于每组测试数据，输出一行，表示最多可以喝的汽水瓶数。如果一瓶也喝不到，输出0。

输入：
3
10
81
0

输出：
1
5
40

```

我的代码，通过测试用例，几点说明：
1. 输入0的处理
2. 初始化n为输入值，以后的n每一轮迭代为n/3的商和余数之和
3. 特殊值n=2，瓶数+1
```python
while 1:
    try:
        n = int(input())
        # 输入0，程序结束
        if n == 0:
            break
        # 一个商quotient，一个输出结果res
        quotient,res = 0,0
        # 只要剩余>=2，就进入循环计算
        while n >= 2:
            # =2时，加一瓶，并跳出
            if n == 2:
                res += 1
                break
            # 迭代n，为商+余数和，同时记录该次的瓶数
            quotient = n//3
            remainder = n%3
            n = quotient + remainder
            res += quotient
        print(res)
    except:
        break
```

讨论区优秀代码，因为在思维上能挖掘出结果，因此代码很简单
```python
while True:
    try:
       a=int(input())
       if a!=0:
           print(a//2)

    except:
        break
```

## Q23：统计每个月兔子总数
```
题目描述
有一只兔子，从出生后第3个月起每个月都生一只兔子，小兔子长到第三个月后每个月又生一只兔子，假如兔子都不死，问每个月的兔子总数为多少？

输入描述:
输入int型表示month

输出描述:
输出兔子总数int型

示例1
输入
9

输出
34
```

经过讨论区提示，写出的代码，说明：
1. 该问题是斐波那契数列，如何从问题中抽象出本质，很难
```
第n个月的兔子数量f(n)，由上个月的已有兔子和本月新出生兔子构成，前者为f(n-1)，后者为f(n-2)，因此有f(n)=f(n-1)+f(n-2)*1
```
2. 使用递归，完成斐波那契求解
3. `while 1 try mycode except break`固定格式
```python
def f(n):
    if n==1 or n==2:
        return 1
    else:
        return f(n-1)+f(n-2)

while 1:
    try:
        n = int(input())
        print(f(n))
    except:
        break
```
4. 使用迭代，a+b=c在下一轮中a为b，b为c
```python
while 1:
    try:
        n = int(input())
        a,b,c=1,1,0
        for i in range(n):
            if i==0 or i==1:
                c=1
            else:
                c = a+b 
                a=b 
                b=c 
        print(c)
    except:
        break
```

## Q24: 计算日期在一年中天数
```
题目描述
根据输入的日期，计算是这一年的第几天。。

详细描述：

输入某年某月某日，判断这一天是这一年的第几天？。

测试用例有多组，注意循环输入

输入描述:
输入多行，每行空格分割，分别是年，月，日

输出描述:
成功:返回outDay输出计算后的第几天; 失败:返回-1                                          
```

我的代码，使用datetime模块，几点说明：
1. 代码要一行行写，本次**写input忘记()**
2. map对象要list
3. date.toordinal()返回日历公元序号,其中公元 1 年 1 月 1 日的序号为 1。两序号相减，记得+1才是天数
```python
import datetime
while 1:
    try:
        date = list(map(int,input().split()))
        my_date = datetime.date(date[0], date[1], date[2])
        print(my_date.toordinal() - datetime.date(date[0],1,1).toordinal()+1)
    except:
        break
```

讨论区代码一，关于strftime()方法说明：
- 对象：适用date\datetime\time对象
- 作用：根据给定的格式将对象转换为字符串
- %j：以补零后的十进制数表示的一年中的日序号，为001, 002, ..., 366
```python
import datetime
while True:
    try:
        print(datetime.datetime(*map(int, input().split())).strftime("%j").lstrip("0"))
    except:
        break
 ```
 
 讨论区代码二，不使用datetime模块，说明：
 1. 判断闰年，if (year%100!=0 and year%4==0) or (year%400==0)
 2. 统计天数，取每个月天数的month-1，如2月3日，只需要取一月的天数+3
 3. 失败-1都未做判断
 ```python
 while 1:
    try:
        date = list(map(int,input().split()))
        year,month,day = date[0],date[1],date[2]
        if (year%100!=0 and year%4==0) or (year%400==0):
            m2 = 29
        else:
            m2 = 28
        m_days = [31,m2,31,30,31,30,31,31,30,31,30,31]

        res = sum(m_days[:month-1])+day
        print(res)
           
    except:
        break
 ```

## Q25: 删除字符串次数最少的字符
```
题目描述
实现删除字符串中出现次数最少的字符，若多个字符出现次数一样，则都删除。输出删除这些单词后的字符串，字符串中其它字符保持原来的顺序。
注意每个输入文件有多组输入，即多个字符串用回车隔开
输入描述:
字符串只包含小写英文字母, 不考虑非法输入，输入的字符串长度小于等于20个字节。

输出描述:
删除字符串中出现次数最少的字符后的字符串。
```

我的代码，说明：
1. 使用Counter统计字符串各字符出现次数，并转化为dic对象
2. 找出次数最少的值，使用min()函数即可
3. 输出符合要求的字符串，使用字符串拼接
```python
from collections import Counter
while 1:
    try:
        s = input()
        stat = dict(Counter(s))
        min_v = min(stat.values())
        del_char = [k for k,v in stat.items() if v==min_v]
        # 输出删除后的字符
        res = ''
        for i in s:
            if not i in del_char:
                res += i
        print(res)
    except:
        break
```

另外一种代码，使用空字符串''替代要删除的字符，但 **遍历字符串又替代字符串逻辑很混乱容易出错，不建议使用**
```python
from collections import Counter
while 1:
    try:
        s = input()
        stat = dict(Counter(s))
        min_v = min(stat.values())
        del_char = [k for k,v in stat.items() if v==min_v]
        # 输出删除后的字符
#         res = ''
#         for i in s:
#             if not i in del_char:
#                 res += i
#         print(res)
        for i in s:
            if i in del_char:
                s = s.replace(i, '')
        print(s)
    except:
        break
```

## Q26:字符串加解密
```
题目描述
1、对输入的字符串进行加解密，并输出。

2、加密方法为：

当内容是英文字母时则用该英文字母的后一个字母替换，同时字母变换大小写,如字母a时则替换为B；字母Z时则替换为a；

当内容是数字时则把该数字加1，如0替换1，1替换2，9替换0；

其他字符不做变化。

3、解密方法为加密的逆过程。

输入
abcdefg
BCDEFGH
输出
BCDEFGH
abcdefg

```

我的代码，添加while句式才能通过测试用例，几点说明：
1. 使用ASCII码来判断字符，ord()函数获得字符的ASCII码，cha()函数将ASCII码转化为字符
2. ord('1')有意义，ord(1)报错
3. 新建res变量，同步遍历原字符串并输出
```python
def en_pw():
    s = input()
    res = ''
    for i in s:
        if ord(i)>=ord('a') and ord(i)<ord('z'):
            res += chr(ord(i)+1).upper()
        elif ord(i)==ord('z'):
            res += 'A'
        elif ord(i)>=ord('A') and ord(i)<ord('Z'):
            res += chr(ord(i)+1).lower()
        elif ord(i)==ord('Z'):
            res += 'a'
        elif ord(i)>=ord('0') and ord(i)<ord('9'):
            res += chr(ord(i)+1)
        elif ord(i)==ord('9'):
            res += '0'
    return res

def de_pw():
    s = input()
    res = ''
    for i in s:
        if ord(i)>ord('a') and ord(i)<=ord('z'):
            res += chr(ord(i)-1).upper()
        elif ord(i)==ord('a'):
            res += 'Z'
        elif ord(i)>ord('A') and ord(i)<=ord('Z'):
            res += chr(ord(i)-1).lower()
        elif ord(i)==ord('A'):
            res += 'z'
        elif ord(i)>ord('0') and ord(i)<=ord('9'):
            res += chr(ord(i)-1)
        elif ord(i)==ord('0'):
            res += '9'
    return res

while 1:
    try:
        print(en_pw())
        print(de_pw())
    except:
        break
```

4. 使用replace会出现异常错误，如加密函数的测试用例为 `abcdefz019` ,输出结果为错误的 `BCDEFGA220`
```python
def en_pw():
    s = input()
    for i in s:
        if ord(i)>=ord('a') and ord(i)<ord('z'):
            s = s.replace(i, chr(ord(i)+1).upper())
        elif ord(i)==ord('z'):
            s = s.replace(i, 'A')
        elif ord(i)>=ord('A') and ord(i)<ord('Z'):
            s = s.replace(i, chr(ord(i)+1).lower())
        elif ord(i)==ord('Z'):
            s = s.replace(i, 'a')
        elif ord(i)>=ord('0') and ord(i)<ord('9'):
            s = s.replace(i, chr(ord(i)+1))
        elif ord(i)==ord('9'):
            s = s.replace(i, '0')
        # return s
    return s

# def de_pw():
#     pass

print(en_pw())
# print(de_pw())
```
5. 讨论区优秀代码，
- 使用ASCII码判断字符，ord(i)返回int型
- 使用mode，-1为加密，-mode为+1
- %10很好的解决了边界数字问题
- 其他非数字、非字母的字符，在else语句中有考虑到
    ```python
    def encodeAndDecode(string,mode):
        result = ''
        for i in string:
            code = ord(i)
            if 48 <= code <= 57:        #字符为数字时
                result += chr(48 + (code - 48 - mode) % 10)
            elif 65 <= code <= 90:      #字符为大写字母时
                result += chr(97 + (code - 65 - mode) % 26)
            elif 97 <= code <= 122:     #字符为小写字母时
                result += chr(65 + (code - 97 - mode) % 26)
            else:                       #其他字符
                result += i
        return result

    try:
        while True:
            print(encodeAndDecode(input(),-1))
            print(encodeAndDecode(input(), 1))
    except Exception:
        pass
    ```

## Q27：字符串按规则排序
```
题目描述
编写一个程序，将输入字符串中的字符按如下规则排序。

规则 1 ：英文字母从 A 到 Z 排列，不区分大小写。

如，输入： Type 输出： epTy

规则 2 ：同一个英文字母的大小写同时存在时，按照输入顺序排列。

如，输入： BabA 输出： aABb

规则 3 ：非英文字母的其它字符保持原来的位置。

如，输入： By?e 输出： Be?y

注意有多组测试数据，即输入有多行，每一行单独处理（换行符隔开的表示不同行）

输入描述:
输入字符串
输出描述:
输出字符串

输入
A Famous Saying: Much Ado About Nothing (2012/8).
输出
A aaAAbc dFgghh: iimM nNn oooos Sttuuuy (2012/8).
```

讨论区代码研究，
```python
while True:
    try:
        a = input()
        # res是最终返回的字符串的列表形式，char是提取的英文字母。
        res, char = [False] * len(a), []
        # 经过这个循环，把相应的非英文字母及其位置存储到了res中。并且把英文字母提取出来了。
        for i, v in enumerate(a):
            if v.isalpha():
                char.append(v)
            else:
                res[i] = v
        # 使用lambda表达式排序，暴力有效。
        char.sort(key=lambda c: c.lower())
        # 将char中对应的字符填到res中。
        for i, v in enumerate(res):
            if not v:
                res[i] = char[0]
                char.pop(0)
        print("".join(res))
    except:
        break
```
1. list.sort(key = lambda x:x.lower())很精妙，既可以忽视字母大小写排序，又可以当字母存在大小写时保持原字母顺序不变（这点是附带的）。
- 该段代码可简写lambda函数，为`list.sort(key = str.lower)`，因为str.lower是通用函数。这里lower不要括号，若添加括号，报错`TypeError: descriptor 'lower' of 'str' object needs an argument`
    ```python
    >>> test3 = ['f','F','A','a','A','a','B']
    >>> test3.sort()
    >>> test3
    ['A', 'A', 'B', 'F', 'a', 'a', 'f']
    >>> test4 = ['f','F','A','a','A','a','B']
    >>> test4.sort(key=lambda x:x.lower())
    >>> test4
    ['A', 'a', 'A', 'a', 'B', 'f', 'F']
    >>> test5 = ['f','F','A','a','A','a','B']
    >>> p = test5
    >>> p.sort()
    >>> p
    ['A', 'A', 'B', 'F', 'a', 'a', 'f']
    >>> test5
    ['A', 'A', 'B', 'F', 'a', 'a', 'f']
    ```
2. 将列表char按顺序插入字符到res中，除了使用以上方法，还可以
```python
        bit = 0
        for i, v in enumerate(res):
            if not v:
                res[i] = char[bit]
                bit += 1
```
3. enumerate()返回对象，可用for遍历。
4. 要输出制定内容，先构造res列表，最后使用join输出。


## Q28:







