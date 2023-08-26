### pydictor生成简单字典

pydictor.py 简单参数说明：

-base 表示密码所使用的字符

-base Type
```d digital [0 - 9]``` # digital [ˈdɪdʒɪtl] 数字的
```L lowercase letters [a - z]``` # 小写字母
```c capital letters [A - Z]```# 大写字母
```-o``` 指定字典存储的位置

###  核心功能字典

#### 1.基础字典

```
python pydictor.py -base L --len 2 3 --encode b64
python pydictor.py -base dLc --len 1 3 -o /awesome/pwd
python pydictor.py -base d --len 4 4 --head Pa5sw0rd --output D:\exists\or\not\dict.txt

参数解释：
-base 基础字典模块  L：为26个小写英文字母  d：为0-9数字  c：26个大写英文字母
--len 定义长度范围 2（min） 3(max) 即为2-3个字符长度
--encode b64 输出结果将会进行base64编码
-o/--output 指定输出路径，无文件夹则自动创建文件夹
--head 添加前缀为Pa5sw0rd
12345678910
```

#### 2.自定义字符集字典

```
python pydictor.py -char "asdf123._@ " --len 1 3 --tail @site.com

参数解释：
-char 自定义字符集字典模块   "asdf123._@"：双引号里是自定义字符
--tail 添加后缀为@site.com
12345
```

#### 3.排列组合字典

```
python pydictor.py -chunk abc 123 "!@#" @ . _ " " --head a --tail @pass --encode md5

参数解释：
-chunk 排列组合字典模块 空格截断为一个参数，例如 -chunk abc 123 "@!"  就有三个参数abc,123,@!进行排列
1234
```

#### 4.语法引擎解析字典

```
python pydictor.py --conf                                  用默认的"/funcfg/build.conf"文件建立字典
python pydictor.py --conf /my/other/awesome.conf
python pydictor.py --conf "[0-9]{6,6}<none>[a-f,abc,123,!@#]{1,1}<none>" --encode md5 --output parsing.txt

参数解析：
--conf 语法引擎解析字典模块，用默认的"/funcfg/build.conf"文件建立字典
--conf /awesome.conf/ 指定根据自定义的配置文件进行生成字典
--conf "" 根据自定义配置语法生成字典
12345678
```

#### 5.规则拓展字典

```
python pydictor.py -extend bob --level 4 --len 4 12
python pydictor.py -extend liwei zwell.com --leet 0 1 2 11 21 --level 2 --len 6 16 --occur "<=10" ">0" "<=2" -o /possbile/wordlist.lst

参数解释：
--extend 规则拓展字典模块  
--level 使用等级
--leet 替换符
1234567
```

#### 6.社会工程学字典

```
                          _ _      _
          _ __  _   _  __| (_) ___| |_ ___  _ __
         | '_ \| | | |/ _` | |/ __| __/ _ \| '__|
         | |_) | |_| | (_| | | (__| || (_) | |
         | .__/ \__, |\__,_|_|\___|\__\___/|_|
         |_|    |___/                         

               Social Engineering Dictionary Builder
                                        Build by LandGrey
----------------------------[ command ]----------------------------
[+]help desc             [+]exit/quit            [+]clear/cls
[+]show option           [+]set option arguments [+]rm option
[+]len minlen maxlen     [+]head prefix          [+]tail suffix
[+]encode type           [+]occur L d s          [+]types L d s
[+]regex string          [+]level code           [+]leet code
[+]output directory      [+]run

----------------------------[ option ]----------------------------
[+]cname                 [+]ename                [+]sname
[+]birth                 [+]usedpwd              [+]phone
[+]uphone                [+]hphone               [+]email
[+]postcode              [+]nickname             [+]idcard
[+]jobnum                [+]otherdate            [+]usedchar
pydictor SEDB>>
123456789101112131415161718192021222324
```

根据以下信息

| information items        | value                                                 |
| ------------------------ | ----------------------------------------------------- |
| chinese name             | 李伟                                                  |
| pinyin name              | liwei                                                 |
| simple name              | lw                                                    |
| simple name              | Lwei                                                  |
| english name             | zwell                                                 |
| birthday                 | 19880916                                              |
| used password            | liwei123456.                                          |
| used password            | liwei@19880916                                        |
| used password            | lw19880916_123                                        |
| used password            | abc123456                                             |
| phone number             | 18852006666                                           |
| used phone number        | 15500998080                                           |
| home phone               | 76500100                                              |
| company phone            | 010-61599000                                          |
| email account            | [33125500@qq.com](mailto:33125500@qq.com)             |
| email account            | [13561207878@163.com](mailto:13561207878@163.com)     |
| email account            | [weiweili@gmail.com](mailto:weiweili@gmail.com)       |
| email account            | [wei010wei@hotmail.com](mailto:wei010wei@hotmail.com) |
| home postcode            | 663321                                                |
| now place postcode       | 962210                                                |
| common nickname          | zlili                                                 |
| id card number           | 152726198809160571                                    |
| student id               | 20051230                                              |
| job number               | 100563                                                |
| father birthday          | 152726195910042816                                    |
| mother birthday          | 15222419621012476X                                    |
| boy/girl friend brithday | 152726198709063846                                    |
| friend brithday          | 152726198802083166                                    |
| pet name                 | tiger                                                 |
| crazy something          | games of thrones                                      |
| special meaning numbers  | 176003                                                |
| special meaning chars    | m0n5ter                                               |
| special meaning chars    | ppdog                                                 |

使用命令

```
python pydictor.py --sedb
set cname liwei
set sname lw Lwei
set ename zwell
set birth 19880916
set usedpwd liwei123456. liwei@19880916 lw19880916_123
set phone 18852006666
set uphone 15500998080
set hphone 76500100 61599000 01061599000
set email 33125500@qq.com
set email 13561207878@163.com
set email weiweili@gmail.com
set email wei010wei@hotmail.com
set postcode 663321 962210
set nickname zlili
set idcard 152726198809160571
set jobnum 20051230 100563
set otherdate 19591004 19621012
set otherdate 19870906 19880208
set usedchar tiger gof gamesthrones 176003 m0n5ter ppdog
1234567891011121314151617181920
```

查看当前配置然后生成字典

```
show
run
12
```

### 0x03 插件型字典

#### 1.一段时间内生日字典

```
python pydictor.py -plug birthday 19800101 20001231 --len 6 8

参数解释：
-plug 插件模块
1234
```

#### 2. 身份证后4/6/8位字典

```
python pydictor.py -plug pid4
python pydictor.py -plug pid6 --encode b64
python pydictor.py -plug pid8 --encode sha1 -o pid8.txt

参数解释：
pid4 身份证后4位
123456
```

#### 3. 网页原始关键词字典

```
python pydictor.py -plug scratch                             用/funcfg/scratch.sites 文件中的多行 url 作为输入
python pydictor.py -plug scratch http://www.example.com
12
```

### 0x04 内置工具

#### 1.字典合并工具

```
python pydictor.py -tool combiner /my/mess/dir

参数解释：
-tool 工具模块
combiner 合并 /my/mess/dir 目录的两个字典，所以需要先将两个字典放在同个文件夹
12345
```

#### 2.字典比较工具

```
python pydictor.py -tool comparer big.txt small.txt
1
```

#### 3.词频统计工具

```
python pydictor.py -tool counter s huge.txt 1000
python pydictor.py -tool counter v /tmp/mess.txt 100
python pydictor.py -tool counter vs huge.txt 100 --encode url -o fre.txt

1234
```

#### 4.字典处理工具

```
python pydictor.py -tool handler raw.txt --tail @awesome.com --encode md5   在字典添加后缀@awesome.com 并通过MD5加密后输出
python pydictor.py -tool handler raw.txt --len 6 16 --occur "" "=6" "<0" --encode b64 -o ok.txt

参数解释：
--occur [字母出现次数的范围] [数字出现次数的范围] [特殊字符出现次数的范围]  示例：--occur "" "=6" "<0"  字母出现0个，数字出现6个，特殊字符小于0个
12345
```

#### 5.安全擦除字典工具

```
python pydictor.py -tool shredder                    擦除当前输出目录下所有字典文件
python pydictor.py -tool shredder base 		         擦除当前输出目录下所有以"base"开头的字典文件
python pydictor.py -tool shredder /data/mess
python pydictor.py -tool shredder D:\mess\1.zip
1234
```

#### 6.合并去重工具

```
python pydictor.py -tool uniqbiner /my/all/dict/
1
```

#### 7.字典去重工具

```
python pydictor.py -tool uniqifer /tmp/dicts.txt --output /tmp/uniq.txt
1
```

#### 8.多字典文件组合工具

```
python pydictor.py -tool hybrider heads.txt some_others.txt tails.txt
1
```

### 0x05 参数表

#### 1.pydictor可以生成的所有字典的类型及其说明

| 归属   | 类别      | 标识符 | 描述                      | 支持功能代号 |
| ------ | --------- | ------ | ------------------------- | ------------ |
| core   | base      | C1     | 基础字典                  | F1 F2 F3 F4  |
| core   | char      | C2     | 自定义字符集字典          | F1 F2 F3 F4  |
| core   | chunk     | C3     | 排列组合字典              | ALL          |
| core   | conf      | C4     | 配置语法生成字典          | ALL          |
| core   | extend    | C5     | 规则扩展字典              | ALL          |
| core   | sedb      | C6     | 社会工程学字典            | ALL          |
| tool   | combiner  | T1     | 字典合并工具              |              |
| tool   | comparer  | T2     | 字典比较相减工具          | ALL          |
| tool   | counter   | T3     | 词频统计工具              | ALL          |
| tool   | handler   | T4     | 筛选处理原有字典工具      | ALL          |
| tool   | uniqbiner | T5     | 先合并后去重工具          | ALL          |
| tool   | uniqifer  | T6     | 字典去重工具              | ALL          |
| tool   | hybrider  | T7     | 多字典文件组合工具        | F1 F2 F3 F4  |
| plugin | birthday  | P1     | 生日日期字典插件          | ALL          |
| plugin | ftp       | P2     | 关键词生成ftp密码字典插件 | ALL          |
| plugin | pid4      | P3     | 身份证后四位字典插件      | ALL          |
| plugin | pid6      | P4     | 身份证后六位字典插件      | ALL          |
| plugin | pid8      | P5     | 身份证后八位字典插件      | ALL          |
| plugin | scratch   | P6     | 网页原始关键词字典插件    | ALL          |

#### 2.字典操作功能及说明对照表

| 功能   | 功能代号 | 说明                                     |
| ------ | -------- | ---------------------------------------- |
| len    | F1       | 定义长度范围                             |
| head   | F2       | 添加前缀                                 |
| tail   | F3       | 添加后缀                                 |
| encode | F4       | 编码或自定义加密方法                     |
| occur  | F5       | 字母、数字、特殊字符出现次数范围筛选     |
| types  | F6       | 字母、数字、特殊字符各种类数范围筛选     |
| regex  | F7       | 正则筛选                                 |
| level  | F8       | 字典级别筛选                             |
| leet   | F9       | 1337 模式                                |
| repeat | F10      | 字母、数字、特殊字符连续出现次数范围筛选 |

#### 3.支持的编码或加密方式

| 方式   | 描述                                       |
| ------ | ------------------------------------------ |
| none   | 默认方式, 不进行任何编码                   |
| b16    | base16 编码                                |
| b32    | base32 编码                                |
| b64    | base64 编码                                |
| des    | des 算法, 需要根据情况修改代码             |
| execjs | 执行本地或远程js函数, 需要根据情况修改代码 |
| hmac   | hmac 算法, 需要根据情况修改代码            |
| md5    | md5 算法输出32位                           |
| md516  | md5 算法输出16位                           |
| rsa    | rsa 算法 需要根据情况修改代码              |
| sha1   | sha-1 算法                                 |
| sha256 | sha-256 算法                               |
| sha512 | sha-512 算法                               |
| url    | url 编码                                   |
| test   | 一个自定义编码方法的示例                   |