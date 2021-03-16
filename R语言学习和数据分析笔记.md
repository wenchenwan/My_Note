# R语言学习和数据分析笔记

在R语言中，使用所有的函数都必须要加括号

## 1. R基本操作

```R
>>getwd()
[1] "C:/Users/wenchenwan/Documents"

>>setwd(dir = "C:/Users/wenchenwan/Documents/R")
#设置工作目录

> list.files()
> dir()
#显示目录下的所有文件

> x <- 3
#赋值运算
> x <<- 3
#强制赋值给一个全局变量
sum()
mean()

> ls()
[1] "x" "y" "Z"
#列出变量

> ls.str()
x :  num 3
y :  num 20
Z :  num 30
#列出变量和值

> str(x)
 num 3
#列出一个变量的值

> ls(all.names = TRUE)
[1] ".Random.seed" "x"            "y"            "Z" 
#类似于ls -a


> rm(x)
> rm(y,z)
#删除变量
> rm(list = ls())
#删除所有变量

> save,image()
#保存工作空间


```

## 2 R包的安装

```R
install.packages("vcd")

>.libPaths()
[1] "D:/software/R-4.0.3/library"
#显示库所在的位置

> library()
#查看有哪些包

> install.packages(c("AER","ca"))
#一次安装多个包

> update.packages()
#包的更新


```

![image-20201230101901644](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201230101928318.png)

![image-20201230101941777](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201230101941777.png)

```R
library(vcd)
require(vcd)
#加载包

search()
#查看哪些包已经加载

help( package="vcd" )
#查看包的帮助文档

library(help = "vcd")
#查看的包的相关信息

ls("package:vcd")
#列出vcd中每个可以使用的函数

data(package="vcd")
#列出vcd中包含的所有数据集

detach("package:vcd")
#移除加载的包

remove.packages("vcd")
#删除已经下载的包
```

### 批量安装包

```R
Rpack <- installed.packages()[,1]
#将第一列的元素保存在Rpack文件中
save(Rpack,file = "Rpack.Rdata")
load("C:/Users/wangtong/Desktop/RData/Rpack.RData")
for (i in Rpack)  install.packages(i)
```

### 显示R软件的帮助菜单

```R
help.start()
help(sum)?
?plot

> args(plot)
function (x, y, ...) 
NULL
#查看plot函数的参数

> example(plot)
#查看例程
example("hist")
example("example")

#作图的例程
demo(graphics)

help(package=ggplot2)
#查看某个包的帮助文档
vignette()
vignette("xts")
#这种帮助文档更规范，并不是每一个都有
??MASS
help.search("heatmap")
#进行本地搜索
??heatmap

apropos("sum")#查找所有包含sum的内容
apropos ("sum",mod = "function")#只显示函数

RSiteSearch("matlab")#进行网络搜索


```

## 3.Excel案例分析

## 4.R内置数据集

```R
help("datasets")
#查看数据集

data()
#列出R所有用到的数据类型

#变量的命名最好不要和数据集的命名重复
rivers <- 123
#再次使用rivers数据集需要重新加载数据
data("rivers")

state<- data.frame(state.name,state.abb,state.area,state.division,state.region)

#如果只想使用数据集而不想加载包
data(Chile,package = "carData")
```

## 5. R语言数据结构

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201230202450748.png" alt="image-20201230202450748" style="zoom:50%;" />

对象

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201230202722677.png" alt="image-20201230202722677" style="zoom: 33%;" />

向量

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201230202853046.png" alt="image-20201230202853046" style="zoom:33%;" />

```R
#创建向量
> x <- c(1,2,3,4,5)
> x
[1] 1 2 3 4 5
#自动调用了print函数
print(x)

y <- c("one","two","three")
z <- c(TRUE,FALSE,T,F)

> c(1:100)
> seq(from=0,to=100)
> seq(1,100)
  [1]   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17  18  19
 [20]  20  21  22  23  24  25  26  27  28  29  30  31  32  33  34  35  36  37  38
 [39]  39  40  41  42  43  44  45  46  47  48  49  50  51  52  53  54  55  56  57
 [58]  58  59  60  61  62  63  64  65  66  67  68  69  70  71  72  73  74  75  76
 [77]  77  78  79  80  81  82  83  84  85  86  87  88  89  90  91  92  93  94  95
 [96]  96  97  98  99 100

> seq(1,100,by=2)
 [1]  1  3  5  7  9 11 13 15 17 19 21 23 25 27 29 31 33 35 37 39 41 43 45 47 49 51
[27] 53 55 57 59 61 63 65 67 69 71 73 75 77 79 81 83 85 87 89 91 93 95 97 99

> seq(1,100,by=2)
> seq(1,100,length.out = 10)#length.out输出序列的长度
 [1]   1  12  23  34  45  56  67  78  89 100

> rep(2,5)
[1] 2 2 2 2 2

> rep(x,each=5)
 [1] 1 1 1 1 1 2 2 2 2 2 3 3 3 3 3 4 4 4 4 4 5 5 5 5 5
```

PS:向量中的元素必须要保持相同的类型

```R
> a <- c(1,2,"one")
> a
[1] "1"   "2"   "one"

> mod(a)
#查看数据的类型

> x <- c(1,2,3,4,5)
> y <- c(6,7,8,9,10)
> x*2+y
[1]  8 11 14 17 20
#向量化编程

> x[x>3]
[1] 4 5

> rep(x,each=5)
 [1] 1 1 1 1 1 2 2 2 2 2 3 3 3 3 3 4 4 4 4 4 5 5 5 5 5
```

## 6. 向量的索引

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20201230210256139.png" alt="image-20201230210256139" style="zoom:50%;" />

```R
> x <- c(1:100)
> length(x)
[1] 100
> x[10]
[1] 10

> x[-19]#不输出第十九个元素

x[4:18]

x[c(1,23,45,67,89)]#分别输出对应位置上的数据
x[c(-2,3,4)]#逻辑矛盾

> y <- c(1:10)
> y[c(T,F,T,T,F,F,T,T,T,F,T)]
[1]  1  3  4  7  8  9 NA
#输出逻辑值为真的值

y[c(T)]
#只要所有元素都为T就输出

> y[c(T,F)]#进行循环的判断
[1] 1 3 5 7 9

> y[y>5]
> y[y>5 & y<9]

z <- c("one","two","three","four","five")
one %in% z
z[z %in% c("one","two")]
   #[1] "one" "two"
########################################################
> z %in% c("one","two")
[1]  TRUE  TRUE FALSE FALSE FALSE

#使用names()函数为向量的每个元素添加名称
> names(y) <- c("one","two","three","four","five","six","seven","eight","nine","ten")
> y
  one   two three  four  five   six seven eight  nine   ten 
    1     2     3     4     5     6     7     8     9    10 
#元素的名称和元素的值相互对应，可以通过元素的名称去访问元素的值

x <- 1:100
x[101] <- 101#增加一个新的元素

v[c(4,5,6)] <- c(4,5,6)#在v4,5,6的位置上添加四五六的值

> v[20]=4
> v
 [1]  1  2  3  4  5  6 NA NA NA NA NA NA NA NA NA NA NA NA NA  4

> append(v,99,after = 5)
 [1]  1  2  3  4  5 99  6 NA NA NA NA NA NA NA NA NA NA NA NA NA  4
#在第五个元素的后边添加99这个元素

rm(v)#删除向量v
y[-c(1,2,3)]#批量删除元素
y <- y[-c(1:3)]

```

## 7. 向量的运算

```R
x <- 1:10
x+1
x-3
x <- x+1
y <- seq(1,100,length.out = 10)
x+y
x*y
x**y#x的y次方
y%%x#取余运算
y%/%x#整除

#在两个向量运算向量长度不相同时候，短的向量会被重复运算，在循环运算中，短的元素循环的次数必须为整数次数

#进行逻辑运算
x>4
[1] FALSE FALSE FALSE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE

> c(1,3,2) %in% c(1,2,2,4,5,6)
[1]  TRUE FALSE  TRUE

> x == y
 [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
```

常用的函数

```R
x <- -5:5
abs(x)
sqrt(x)
log(16,base=2)#没有base参数为自然对数
log10(10)
exp(x)#计算向量的指数
ceiling (c(-2.3,3.1415))#返回不小于x的最小整数
floor(c(-2.3,3.1415))#返回不大于x的最小整数
trunc(c(-2.3,3.1415))#返回x的整数部分
round (c(-0.618,3.1415),digits=2)#进行四舍五入，digit控制保留小数位数
signif (c(-0.0618,3.1415),digits=3)#进行四舍五入，digit控制有效位数
sin(x);cos(x);tan(x)
```

统计函数

```R
vec <- 1:100
sum(vec)
max(vec)
min(vec)
range(vec)#返回最大值和最小值
mean(vec)
var(vec)#返回向量的方差
round (var(vec),digits=2)
sd(vec)#返回向量的标准差    
prod(vec)#返回向量连乘的积
median(vec)#计算中位数
quantile(vec)#计算峰位数
quantile (vec,c(0.4,0.5,0.8))#计算四分位数，中分位数，八分位数
```

得到索引函数

```R
#get index
t <- c (1,2,2,5,7,9,6)
which.max (t) 
which.min(t)
which(t==7)
which(t>5)
t[which (t>5)]
#均返回索引值
```

## 8. 矩阵和数组

```R
#用向量创建矩阵
> x
 [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
> m <- matrix(x,nrow = 4,ncol = 5)
> m
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    5    9   13   17
[2,]    2    6   10   14   18
[3,]    3    7   11   15   19
[4,]    4    8   12   16   20
#在只提供一个参数的时候，另外一个参数会自动进行分配
> m <- matrix(x,nrow = 4)
> m
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    5    9   13   17
[2,]    2    6   10   14   18
[3,]    3    7   11   15   19
[4,]    4    8   12   16   20
> m <- matrix(x,nrow = 5)
> m
     [,1] [,2] [,3] [,4]
[1,]    1    6   11   16
[2,]    2    7   12   17
[3,]    3    8   13   18
[4,]    4    9   14   19
[5,]    5   10   15   20

> m <- matrix(x,4,4)
> m
     [,1] [,2] [,3] [,4]
[1,]    1    5    9   13
[2,]    2    6   10   14
[3,]    3    7   11   15
[4,]    4    8   12   16
#矩阵的列必须是数组元素的倍数

#给矩阵的行和列命名
> dim <- c("R1","R2","R3","R4")
> dim1 <- c("R1","R2","R3","R4")
> dim2 <- c("C1","C2","C3","C4")
> dimnames(m)<-list(dim1,dim2)
> m
   C1 C2 C3 C4
R1  1  5  9 13
R2  2  6 10 14
R3  3  7 11 15
R4  4  8 12 16

#dim设置矩阵的维数
dim(x) <- c(2,2,5)#三维数组

z <- array(1:24, c(2,3,4), dimnames=list(dim1, dim2, dim3))
#创建数组array


> x <- 1:20
> m <- matrix(x,nrow = 4,ncol = 5,byrow = TRUE)
> m
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    2    3    4    5
[2,]    6    7    8    9   10
[3,]   11   12   13   14   15
[4,]   16   17   18   19   20
> m <- matrix(x,nrow = 4,ncol = 5,byrow = FALSE)
> m
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    5    9   13   17
[2,]    2    6   10   14   18
[3,]    3    7   11   15   19
[4,]    4    8   12   16   20


#Using matrix subscripts
m <- matrix(x,nrow = 4,ncol = 5)
m[1,2]
m[1,c(2,3,4)]
m[c(2,4),c(2,3)]
m[2,]#第二行
m[,2]#第二列
m[2]#返回对应的列
m[-1,2]#去除第一行取出第二行

```

矩阵的运算

```R
#Matrix peration
m+1
m*2
m+m
n <- matrix(1:20,5,4)
m+n
colSums(m)
rowSums(m)#行
colMeans(m)
rowMeans(m)

n <- matrix (1:9,3,3)
t <- matrix (2:10,3,3)
n*t#内积
n%*%t#外积，线代里面的矩阵乘法

diag(n)
diag(m)#获得矩阵元素的对角元素，组成向量

a <- matrix(rnorm(16),4,4)#生成4*4的随机矩阵
solve(a)#计算a的逆矩阵
eigen(a)#计算a的特征值和特征向量
dist(a)#计算a的距离
```

## 9. 列表

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210101101805734.png" alt="image-20210101101805734" style="zoom: 33%;" />

向量与列表

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210101102051137.png" alt="image-20210101102051137" style="zoom:33%;" />

列表的创建

```R
a <- 1:20
b <- matrix(1:24,4,6)
c=mtcars
d <- "This is a test list"
mlist <- list(a,b,c,d)
mlist <- list(first=a,second=b,third=c,fourth=d)#为每一个元素命名
#用list创建列表
```

列表的访问

```R
mlist[1]
mlist[c(1,4)]#访问第一个和第四个元素
state.center[c("x","y")]

#使用名称访问对象
mlist$first
state.center$x

> class(mlist[2])#对象的一部分
[1] "list"
> class(mlist[[2]])#元素本身的类型
[1] "matrix" "array" 

> mlist[[5]] <- c(1,2,3,4)#给列表赋值
> mlist[[5]] <- NULL#清空元素的值
```

## 10. 数据框 

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210101112810491.png" alt="image-20210101112810491" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210101113155748.png" alt="image-20210101113155748" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210101113217931.png" alt="image-20210101113217931" style="zoom:50%;" />

```R
#内置的数据框
iris
mtcars
rock

#数据框的创建
state <- data.frame(state.name,state.abb,state.region,state.x77)

#访问数据集的内容
state[1]
state[c(2,4)]
state[,"state.abb"]#利用行和列名直接取出行和列
state["Alabama",]

state$state.name#使用美元符号直接取出数据框中的元素

plot(women$height,women$weight)#绘制身高体重的散点图
lm (weight ~height,data = women)#进行线性回归

> attach(women)
> height
 [1] 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72
> weight
 [1] 115 117 120 123 126 129 132 135 139 142 146 150
[13] 154 159 164
#使用attach就加载到工作空间，而不需要使用$符号
> detach(women)#使用结束，使用detach函数取消加载

#使用with访问数据框数据
> with(women,{height})
 [1] 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72
> with(women,{weight})
 [1] 115 117 120 123 126 129 132 135 139 142 146 150
[13] 154 159 164
```

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210101115334824.png" alt="image-20210101115334824" style="zoom:50%;" />

单中括号只是取出一节火车，但是它还是火车，但是使用双中括号则是取出一截车皮，已经不在是火车了

## 11. 因子

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210101121634844.png" alt="image-20210101121634844" style="zoom:50%;" />

```R
> mtcars$cyl#可以视为一个因子
 [1] 6 6 4 6 8 6 8 4 4 6 6 8 8 8 8 8 8 4 4 4 4 8 8 8 8
[26] 4 4 4 8 6 8 4
> table(mtcars$cyl)#统计频数
 4  6  8 
11  7 14 

#创建因子
> f <- factor(c("red","red","green","red","blue","green","blue","blue"))
> f
[1] red   red   green red   blue  green blue  blue 
Levels: blue green red

#创建的因子有了顺序
week <- factor(c("Mon","Fri","Thu","Wed","Mon","Fri","Sun"),order = TRUE,
               levels = c("Mon","Tue","Wed","Thu","Fri","Sat","Sun"))

> fcyl <- factor(mtcars$cyl)
> plot(mtcars$cyl)#散点图
> plot(fcyl)#条形图

#对元素进行有规律的分组，十个为一组进行分析
num <- c(1:100)
cut (num,c(seq(0,100,10)))

#内置的因子类的数据
state.division
state.region
```

## 12. 缺失数据

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210101123755613.png" alt="image-20210101123755613" style="zoom:50%;" />

 

```R
x <- 1:5
x[10] <- 100
x
1+NA
NA==0
a <- c(NA,1:49)
sum(a)#有NA计算的结果为NA
mean(a)
sum(a,na.rm = TRUE)
mean(a,na.rm = TRUE)#数组的元素个数减一，来计算平均值

> is.na(a)#在为NA的元素的位置上为TRUE
 [1]  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
[15] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
[29] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
[43] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE

#计算行和列的和
> rowSums(is.na(sleep))
 [1] 2 0 2 3 0 0 0 0 0 0 0 0 2 2 0 0 0 0 1 1 2 0 0 2 0 2 0 0 0 2 3 0 0 0 1 1 0 0 0 0 2 0 0
[44] 0 0 0 2 0 0 0 0 0 2 0 2 1 0 0 0 0 0 3
> colSums(sleep)
 BodyWgt BrainWgt     NonD    Dream    Sleep     Span     Gest     Pred      Exp   Danger 
12324.98 17554.32       NA       NA       NA       NA       NA   178.00   150.00   162.00

#计算行和列为NA的对象个数
colSums(is.na(sleep))
rowSums(is.na(sleep))

#移除向量的NA元素
c <- c(NA,1:20,NA,NA)
d <- na.omit(c)
na.omit(sleep)

#处理数据框的时候，na.omit将会把包含有NA的行全部删除
na.omit(sleep)

1/0
-1/0

> 0/0
[1] NaN

is.nan(0/0)
is.infinite(1/0)
```

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210101165139928.png" alt="image-20210101165139928" style="zoom: 33%;" />

## 13. 字符串

```R
nchar ("Hello World")#统计字符的个数
#也可以统计字符数组的的元素个数
> nchar(month.name)
 [1] 7 8 5 5 3 4 4 6 9 7 8 8
> length(month.name)#计算数组的长度
[1] 12

> paste("Everybody","loves","stats")#将多个向量合并为一个，默认使用空格链接
paste("Everybody","loves","stats"，sep="-")#指定链接的方式


> names <- c("Moe","Larry","Curly")
> paste(names,"love stats")#将names中的每一个元素都和后边的字符串链接

Mon <- substr(month.name,start = 1,stop = 3)
substr(month.name,1,3)#取数组字符串的前三个字符
[1] "Jan" "Feb" "Mar" "Apr" "May" "Jun" "Jul" "Aug" "Sep" "Oct" "Nov" "Dec"
> toupper(Mon)#转换为大写
> tolower(Mon)#转换为小写

#利用正则表达式实现首字母大写
> gsub("^(\\w)", "\\U\\1",tolower(Mon),perl = T)
 [1] "Jan" "Feb" "Mar" "Apr" "May" "Jun" "Jul" "Aug" "Sep" "Oct" "Nov" "Dec"
> gsub("^(\\w)", "\\L\\1",toupper(Mon),perl = TRUE)#实现首字母小写

#grep字符串匹配
x <- c("b","A+","AC")
grep ("A+",x,fixed=TRUE)#单纯的查找字符
grep ("A+",x,fixed=FALSE)#按正则表达式查找
match("AC",x)
path <- "/usr/local/bin/R"
strsplit(path,"/")#按/分割字符串
strsplit(c(path,path),"/")

#outer函数生成两个数组的笛卡尔积
face <- 1:13
suit <- c("spades","clubs","hearts","diamonds")
outer(suit,face,FUN=paste)
outer(suit,face,FUN=paste,sep="-")

> outer(suit,face,FUN=paste,sep="-")
     [,1]         [,2]         [,3]         [,4]         [,5]         [,6]        
[1,] "spades-1"   "spades-2"   "spades-3"   "spades-4"   "spades-5"   "spades-6"  
[2,] "clubs-1"    "clubs-2"    "clubs-3"    "clubs-4"    "clubs-5"    "clubs-6"   
[3,] "hearts-1"   "hearts-2"   "hearts-3"   "hearts-4"   "hearts-5"   "hearts-6"  
[4,] "diamonds-1" "diamonds-2" "diamonds-3" "diamonds-4" "diamonds-5" "diamonds-6"
     [,7]         [,8]         [,9]         [,10]         [,11]         [,12]        
[1,] "spades-7"   "spades-8"   "spades-9"   "spades-10"   "spades-11"   "spades-12"  
[2,] "clubs-7"    "clubs-8"    "clubs-9"    "clubs-10"    "clubs-11"    "clubs-12"   
[3,] "hearts-7"   "hearts-8"   "hearts-9"   "hearts-10"   "hearts-11"   "hearts-12"  
[4,] "diamonds-7" "diamonds-8" "diamonds-9" "diamonds-10" "diamonds-11" "diamonds-12"
     [,13]        
[1,] "spades-13"  
[2,] "clubs-13"   
[3,] "hearts-13"  
[4,] "diamonds-13"
```

## 14. 时间和日期

```R
> class(presidents)
[1] "ts"#时间序列

Sys.Date()#获取系统的时间
a="2017-01-01"
as.Date(a)#将字符串a转化为时间
b=as.Date(a,format="%Y-%m-%d")#规定转换的格式，指定a的格式
class(b)

seq(as.Date("2017-01-01"),as.Date("2017-07-05"),by=5)#创建连续的时间点

sales <- round(runif(48,min=50,max=100))
ts(sales,start = c(2010,5),end = c(2014,4),frequency = 1)#以年为单位
ts(sales,start = c(2010,5),end = c(2014,4),frequency = 4)#以季度为单位
ts(sales,start = c(2010,5),end = c(2014,4),frequency = 12)#以月为单位
#runif取随机数
#round取整数
```

## 15. 获取数据

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210101175425503.png" alt="image-20210101175425503" style="zoom:50%;" />

```R
#手动创建一个数据框
patientID <- c(1, 2, 3, 4)
admdate <- c("10/15/2009","11/01/2009","10/21/2009","10/28/2009")
age <- c(25, 34, 28, 52)
diabetes <- c("Type1", "Type2", "Type1", "Type1")
status <- c("Poor", "Improved", "Excellent", "Poor")
data <- data.frame(patientID, age, diabetes, status)
#使用edit函数来进行手动编辑填写数据，先定义数据框的类型，然后进行数据的填充
data2 <- data.frame(patientID=character(0), age=numeric(0),
                    diabetes=character(), status=character())
data2 <- edit(data2)
#也可以直接使用fix函数来进行数据的填充
> fix(data2)
```

### 1. 读取外部文件的数据

> ```R
> X = read.table("RData\\input.txt")#将文本中的表格读入R中
> head(X)#显示头部的六行
> tail(X，n=10)#显示尾部的六行,n为显示的参数
> x <- read.table ("input.csv",sep=",")#sep指定分隔符
> 
> x <- read.table ("input.csv",sep=",",header = T)#header=T将第一行变量看作变量名称
> x <- read.table ("input 1.txt",sep=",",header = T,skip = 5)#跳过前五行，从第六行开始读取信息
> x <- read.table ("input.csv",sep=",",header = T,nrows = 100)#读取文件的前一百行
> x <- read.table ("input.csv",sep=",",header = T,skip = 50,nrows = 100)#从第五十行读取200行内容
> x <- read.table ("input.csv",sep=",",header = T,skip = 50,nrows = 100,
>                  na.strings = " ")#na.strings是为确实值的符号形式
> x <- read.table ("input.csv",sep=",",header = T,skip = 50,nrows = 100,
>                  stringsAsFactors = F)#默认条件下字符串会被转换为因子，设置为F就不会进行转换
> ```

### 2. 读取网络文件

```R
x <- read.table("https://codeload.github.com/mperdeck/LINQtoCSV/zip/master",header = TRUE)

require(XML)
readHTMLTable("https://en.wikipedia.org/wiki/World_population",which=3)#读取网页

help(package = "foreign")#查看foreign的帮助文档

RSiteSearch("Matlab")#搜索MATLAB相关的包
x <- read.table("clipboard")#读取剪切板上的文件
x <- readClipboard()

x <- readLines("input.txt",n=100)#读取100行
x <- read.table(gzfile("input.txt.gz"))#直接读取压缩文件

x <- scan("scan.txt",what=list (character(3),numeric(0),numeric(0)))#括号的（0）（3）分别是什么
x <- scan("scan.txt",what=list (X1=character(3),X2=numeric(0),X3=numeric(0)))#读取scan.txt中的字符数字

```

通过访问外部数据库实现访问数据

读取R数据格式

```R
?iris
head(iris)
getwd()
dir()
saveRDS(iris,file="iris.RDS")
rdsdata <- readRDS("C:/Users/wangtong/Desktop/RData/iris.RDS")
#Write RData file
load(file = "C:/Users/wangtong/Desktop/RData/Ch02.R")
save(iris,iris3,file = "iris.Rdata")
save.image()
```

读写操作excel文件

```R
read.csv("input.csv",header = T)
install.packages("XLConnect")
library(XLConnect)
?loadWorkbook
#Two step Read Excel File
ex <- loadWorkbook ("data.xlsx")
edata <- readWorksheet(ex,1)
head(edata)
edata <- readWorksheet(ex,1,startRow=0,starCol=0,endRow=50,endCol=3)

#One step Read Excel File
readWorksheetFromFile ("data.xlsx",1,startRow=0,starCol=0,
                       endRow=50,endCol=3,header=TRUE)
#Four step Wtire Excel File
wb <- loadWorkbook("file.xlsx",create=TRUE)
createSheet(wb,"Sheet 1")
writeWorksheet(wb,data=mtcars,sheet = "Sheet 1")
saveWorkbook()

#One step Wtire Excel File
writeWorksheetToFile("file.xlsx",data = mtcars,sheet = "Sheet 1")
vignette("XLConnect")

#Packages xlsx
install.packages("xlsx")
library(xlsx)
rdata <- read.xlsx("data.xlsx",n=1,startRow = 1,endRow = 100)
write.xlsx(rdata,file = "rdata.xlsx",sheetName = "Sheet 1",append = F)
help(package="xlsx")
```

## 16. 数据格式转换

### 格式转换1

```R
library(xlsx)
cars32 <- read.xlsx("mtcars.xlsx",sheetIndex = 1,header = T)
is.data.frame(cars32)#判断是否为数据框格式数据
is.data.frame(state.x77)#state.x77是矩阵，不是数据框
dstate.x77 <- as.data.frame(state.x77)#将矩阵转换为数据框格式
is.data.frame(dstate.x77)
as.matrix(data.frame(state.region,dstate.x77))#数据框转换为矩阵
#当矩阵转换为数据框的时候，所有的元素都会被转换为字符串
as.vector(dstate.x77)
as.factor(dstate.x77)#不能转换为因子和向量
methods(is)
methods(as)#使用method来查看有哪些函数可以来被使用
x <- state.abb#向量
dim(x) <- c(5,10)#转换为一个5*10的矩阵
x <- state.abb
as.factor(x)
as.list(x)
state <- data.frame(x,state.region,state.x77)
state$Income#访问列
state["Nevada",]#访问行注意要有逗号
unname(state)#去除名字
unlist(state)#转化为向量
```

### 格式转换2

```R
getwd()
list.files()
who <- read.csv("WHO.csv",header = T)
who1 <- who[c(1:50),c(1:10)]#取子集，前五十行，前十列
who2 <- who[c(1,3,5,8),c(2,14,16,18)]
who3 <- who[which(who$Continent==7),]#选择列continent = 7的信息
who4 <- who[which(who$CountryID>50 & who$CountryID<=100),]
#subset
?subset
who3=subset(who,who$CountryID>50 & who$CountryID<=100)
?sample
x <- 1:100
sample(x,30)#随机抽样30个数据
sample(x,30,replace = T)#无放回抽取
sample(x,60,replace = F)#有放回的抽取
sort(sample(x,60,replace = T))
who3=who[sample(who$CountryID,30,replace = F),]#在数据框中随机抽样

mtcars[,-1:-5]#删除固定行
mtcars$mpg <- NULL#清空列的数据
USArrests
data.frame(state.division,USArrests)#生成一个数据集
cbind(USArrests,state.division)#合并数据集的列
data1 <- head(USArrests,20)
data2 <- tail(USArrests,20)
data3 <- head(cbind(USArrests,state.division),20)
rbind(data3,data2)#合并数据的行

data1 <- head(USArrests,30)
data2 <- tail(USArrests,30)
data4 <- rbind(data1,data2)
rownames(data4)#查看数据的列名
length(rownames(data4))
duplicated(data4)#去除数据内的重复数据
data4[duplicated(data4),]#删除不重复的行，取出重复的部分
data4[!duplicated(data4),]#不取消数据重复选项
unique (data4)#非重复的值，每一行都为特异
```

**合并数据集合的列很容易，合并数据集的行要求必须要求新数据与原来的数据要有相同的列命**

### 格式转换3 

```R
mtcars
sractm <- t(mtcars)#t函数就是转置的意思
letters
rev(letters)#将向量反转过来
womenwomen[rev(rownames(women)),]#翻转数据框的内容
transform(women, height = height*2.54)#进行数据格式转换
data.frame(women$height*2.5,women$weight)#数据转化的效率太低


transform(women, cm = height*2.54)#在women中添加一组的cm列

#数据框的排序
sort(rivers)#对向量进行排序
sort(state.name)
rev(sort(rivers))
mtcars[sort(rownames(mtcars)),]#对数据框进行排序
sort(rivers)#返回排序结果
order (rivers)#返回向量所在的位置
mtcars[order(mtcars$mpg),]
mtcars[order(-mtcars$mpg),]
order(mtcars$mpg,mtcars$disp)#在mpg相同的条件下，disp小的排在前边
```

数据格式转换4

```R
#对数据框进行计算
WorldPhones
worldphones <- as.data.frame(WorldPhones)
rs <- rowSums(worldphones)
cm <- colMeans(worldphones)
total <- cbind(worldphones,Total=rs)#添加一列数据
rbind(total,cm)#再添加一行数据
```

apply系列函数

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210104104929710.png" alt="image-20210104104929710" style="zoom:50%;" />

```R
apply(WorldPhones,MARGIN = 1,FUN = sum)#MARGIN=1代表行
apply(WorldPhones,MARGIN = 2,FUN = mean)#MARGIN=2代表列

state.center
lapply(state.center,FUN = length)#返回列表
sapply(state.center,FUN = length)#返回向量或者矩阵

tapply(state.name,state.division,length)#state.division是一个因子，state.name是数据集，length来统计各个因子的数目

```

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210104111218337.png" alt="image-20210104111218337" style="zoom:50%;" />

```R
#对数据进行中心化处理
x <- c(1,2,3,6,3)
mean(x)
x - mean(x)

sd(x)#求序列x的标准差
(x -mean(x))/sd(x)

scale(state.x77,center = T)#中心化
scale(state.x77,scale = T)#标准化
x <- scale(state.x77,scale = T,center = T)
heatmap(x)
```

### 格式转换4

```R
x <- data.frame(k1 = c(NA,NA,3,4,5), k2 = c(1,NA,NA,4,5),
                data = 1:5)
y <- data.frame(k1 = c(NA,2,NA,4,5), k2 = c(NA,NA,3,4,5),
                data = 1:5)
merge(x, y, by = "k1")#根据k1的交集来合并
merge(x, y, by = c("k1","k2")) #根据看(k1,k2)的交集合并
install.packages("reshape2")
library(reshape2)
?melt
?dcast
?acast
names(airquality) <- tolower(names(airquality))
head(airquality)

aql <- melt(airquality) 
head(aql)

aql <- melt(airquality, id.vars = c("month", "day"))#将月份和天数作为id变量
head(aql)

aqw <- dcast(aql, month + day ~ variable)#~表示相关性
head(aqw)

head(airquality) 
dcast(aql, month ~ variable)#按月份进行统计
dcast(aql, month ~ variable, fun.aggregate = mean, na.rm = TRUE)#fun.aggregate指定要进行操作的函数名，na.rm移除缺失值
```

### 格式转换Tidyr

```R
install.packages(c("tidyr","dplyr"))
tdata <- mtcars[1:10,1:3]
tdata <- data.frame(name=rownames(tdata),tdata)
gather(tdata,key = "Key",value = "Value",cyl,disp,mpg)
                name  Key Value
1          Mazda RX4  cyl   6.0
2      Mazda RX4 Wag  cyl   6.0
3         Datsun 710  cyl   4.0
4     Hornet 4 Drive  cyl   6.0
5  Hornet Sportabout  cyl   8.0
...


gather(tdata,key = "Key",value = "Value",cyl:disp)#将cyl和disp数据放到同一列
                name  mpg  Key Value
1          Mazda RX4 21.0  cyl   6.0
2      Mazda RX4 Wag 21.0  cyl   6.0
3         Datsun 710 22.8  cyl   4.0
4     Hornet 4 Drive 21.4  cyl   6.0
5  Hornet Sportabout 18.7  cyl   8.0
6            Valiant 18.1  cyl   6.0
7         Duster 360 14.3  cyl   8.0
8          Merc 240D 24.4  cyl   4.0
9           Merc 230 22.8  cyl   4.0
10          Merc 280 19.2  cyl   6.0
11         Mazda RX4 21.0 disp 160.0
12     Mazda RX4 Wag 21.0 disp 160.0
13        Datsun 710 22.8 disp 108.0
14    Hornet 4 Drive 21.4 disp 258.0
...



gather(tdata,key = "Key",value = "Value",mpg,cyl,-disp)#disp单独为一列不进行转换
spread(gdata,key = "Key",value = "Value")#与gather的功能相反
df <- data.frame(x = c(NA, "a.b", "a.d", "b.c"))
separate(df,col=x,into = c("A","B"))#按照连字符拆分（自动识别）
df <- data.frame(x = c(NA, "a.b-c", "a-d", "b-c"))
separate(df,x,into = c("A","B"),sep = "-")
unite(x,col ="AB",A,B,sep="-")#使用连字符进行链接
```

格式转换dplyr

```R
library(dplyr)
ls("package:dplyr")
dplyr::filter (iris,Sepal.Length >7)
dplyr::distinct(rbind(iris[1:10,],iris[1:15,]))#去除重复的信息
dplyr::slice(iris,10:15)#取出数据的任意行
dplyr::sample_n(iris,10)#随机抽取十行
dplyr::sample_frac(iris,0.1)#按比例随机选取
dplyr::arrange(iris,Sepal.Length)#进行排序
dplyr::arrange(iris,desc(Sepal.Length))#相反的方向排序
summarise(iris,avg=mean(Sepal.Length))#计算花萼的平均长度
summarise(iris,sum=sum(Sepal.Length))#计算花萼的总长度

head(mtcars,20) %>% tail()#前一个函数的输出是后一个函数的输入
dplyr::group_by(iris,Species)#对数据iris进行分类
iris %>% group_by(Species) %>% summarise(avg=mean(
  Sepal.Width)) %>% arrange(avg)
dplyr::mutate(iris,new=Sepal.Length+Petal.Length)#增加一个新的列
```

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210104195329242.png" alt="image-20210104195329242" style="zoom:67%;" />

```R
#对多表进行操作
a=data.frame(x1=c("A","B","C"),x2=c(1,2,3))
b=data.frame(x1=c("A","B","D"),x3=c(T,F,T))
dplyr::left_join(a,b,by="x1")#左链接
dplyr::right_join(a,b,by="x1")
dplyr::full_join(a,b,by="x1")#取并集
dplyr::semi_join(a,b,by="x1")#取交集
dplyr::anti_join(a,b,by="x1")#输出a与b的补集

first <- slice(mtcars,1:20)#取前20行
mtcars <- mutate(mtcats,Model=rownames(mtcars))
first <- slice(mtcars,1:20)
second <- slice (mtcars,10:30)
intersect(first, second)#取交集
union_all(first, second)#取并集
union(first, second)#取非冗余的并集
setdiff(first, second)#取first的补集
setdiff(second, first) #取second的补集
```

## 17. R函数

选项和参数（选或是不选，参数就是选的多少 ）

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210104203338742.png" alt="image-20210104203338742" style="zoom: 33%;" />

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210104205837236.png" alt="image-20210104205837236" style="zoom: 33%;" />

<img src="https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210104205900649.png" alt="image-20210104205900649" style="zoom:33%;" />



![image-20210104210505537](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210104210505537.png)



### 数学统计函数

```R
round(rnorm(n=100,mean = 15,sd=2))#round取整
?Geometric#几何分布
?Hypergeometric#超几何分布
x=rnorm(n=100,mean = 15,sd=2)
qqnorm(x)#绘制正态分布图
runif(1)#生成随机数
runif(50)
runif(10)*10
runif(50,min=1,max=100)
dgamma(c(1:9),shape = 2,rate = 1)#随机生成gama分布的概率密度
set.seed(666)#就会生成相同的随机数
runif(50)
runif(50)
set.seed(666)
runif(50)

```

### 描述性统计函数

```R
myvars <- mtcars[c("mpg", "hp", "wt", "am")]
summary(myvars)
      mpg              hp              wt              am        
 Min.   :10.40   Min.   : 52.0   Min.   :1.513   Min.   :0.0000  
 1st Qu.:15.43   1st Qu.: 96.5   1st Qu.:2.581   1st Qu.:0.0000  
 Median :19.20   Median :123.0   Median :3.325   Median :0.0000  
 Mean   :20.09   Mean   :146.7   Mean   :3.217   Mean   :0.4062  
 3rd Qu.:22.80   3rd Qu.:180.0   3rd Qu.:3.610   3rd Qu.:1.0000  
 Max.   :33.90   Max.   :335.0   Max.   :5.424   Max.   :1.0000 

fivenum(myvars$hp)
(minimum, lower-hinge, median, upper-hinge, maximum)

#Descriptive stats via describe (Hmisc)
install.packages("Hmisc")
library(Hmisc)
myvars <- c("mpg", "hp", "wt")
describe(mtcars[myvars])#返回数值的统计相关信息


#Descriptive stats via stat.desc (pastecs)
install.packages("pastecs")
library(pastecs)
stat.desc(mtcars[myvars])#计算很多的相关统计量

> stat.desc(mtcars[myvars])
                     mpg           hp          wt
nbr.val       32.0000000   32.0000000  32.0000000
nbr.null       0.0000000    0.0000000   0.0000000
nbr.na         0.0000000    0.0000000   0.0000000
min           10.4000000   52.0000000   1.5130000
max           33.9000000  335.0000000   5.4240000
range         23.5000000  283.0000000   3.9110000
sum          642.9000000 4694.0000000 102.9520000
median        19.2000000  123.0000000   3.3250000
mean          20.0906250  146.6875000   3.2172500
SE.mean        1.0654240   12.1203173   0.1729685
CI.mean.0.95   2.1729465   24.7195501   0.3527715
var           36.3241028 4700.8669355   0.9573790
std.dev        6.0269481   68.5628685   0.9784574
coef.var       0.2999881    0.4674077   0.3041285
stat.desc(mtcars,basic = TRUE,desc = TRUE,norm = TRUE)

#Descriptive stats via describe (psych)
library(psych)
describe(mtcars[myvars],trim = 0.1)#去除最高和最低的10%的部分
Hmisc::describe(mtcars)#后入为主

#Descriptive stats by group with aggregate
library(MASS)
aggregate(Cars93[c("Min.Price","Price","Max.Price","MPG.city")],
          by=list(Manufacturer=Cars93$Manufacturer),mean)#按照制造商进行分类，求平均值
aggregate(Cars93[c("Min.Price","Price","Max.Price","MPG.city")],
          by=list(Manufacturer=Cars93$Manufacturer),sd)#求标准差
aggregate(Cars93[c("Min.Price","Price","Max.Price","MPG.city")],
          by=list(Origin=Cars93$Origin),mean#按照制造产地进行分类
aggregate(Cars93[c("Min.Price","Price","Max.Price","MPG.city")],
          by=list(Origin=Cars93$Origin),sd)
aggregate(Cars93[c("Min.Price","Price","Max.Price","MPG.city")],
          by=list(Origin=Cars93$Origin,Manufacturer=Cars93$Manufacturer),mean)
#Descriptive stats by group with by
mystats <- function(x){
  m <- mean(x)
  s <- sd(x)
  return(c(mean=m, stdev=s))
}
dstats <- function(x)sapply(x,mystats)
by(mtcars[myvars], mtcars$am, dstats)

#Descriptive stats by group via summaryBy
install.packages("doBy")
library(doBy)
summaryBy(mpg+hp+wt~am, data=mtcars, FUN=mystats)
#不同的变量用加号表示，波浪线后边的是分组变量

#Descriptive stats by group via describe.by (psych)
library(psych)
describeBy(mtcars[myvars], list(am=mtcars$am))
```

