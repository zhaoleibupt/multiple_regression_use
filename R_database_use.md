# R_databasepackage_use


#1.use R to connect database(RMySQL,RODBC)


you should require packages of RMySQL and RODBC  first

```r
library(RMySQL)
```

```
## Loading required package: DBI
```

```r
library(RODBC)
```


#2.RMySQL(warning:this is not suitable for chinese character(you can read the table including chinese character,but not write the table including chinese )


###(1)we use the function "dbConnect"" to connect the mysql database

```r
conn<-dbConnect(MySQL(),dbname="php_one",username="root",password="zhaolei19930410")
```

###(2)the function "dbListTable" show the tables of dbname that we connect above

```r
dbListTables(conn)
```

```
## [1] "data"  "data1" "test1" "test2" "test3" "test4" "test5" "test6"
```

###(3)we can write data into database

```r
id<-1:10
name<-letters[1:10]
data<-rnorm(10)
dat<-data.frame(id=id,name=name,data=data)
dbWriteTable(conn,"test7",dat)
```

```
## [1] TRUE
```
###(4)we can select data from tables that we write above

```r
dbSendQuery(conn,'SET NAMES gbk')   # this is for characters in R,it's important, otherwise,you will not see the correct answer
```

```r
res=dbSendQuery(conn,"select * from test7")
dat=fetch(res)
dat
```

```
##    row_names id name        data
## 1          1  1    a  0.02504362
## 2          2  2    b -0.36419842
## 3          3  3    c -1.90839687
## 4          4  4    d -0.30164769
## 5          5  5    e -0.02129595
## 6          6  6    f -1.29134094
## 7          7  7    g -0.03734708
## 8          8  8    h -0.68728315
## 9          9  9    i -2.09126178
## 10        10 10    j -0.53668914
```

```r
dbDisconnect(conn)
```

```
## Warning: Closing open result sets
```

```
## [1] TRUE
```








