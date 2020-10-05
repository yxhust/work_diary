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




