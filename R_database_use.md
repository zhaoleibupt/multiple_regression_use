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
## 1          1  1    a -0.69218963
## 2          2  2    b  1.39087751
## 3          3  3    c -0.21863507
## 4          4  4    d  1.52916430
## 5          5  5    e -0.73909654
## 6          6  6    f  0.22878470
## 7          7  7    g -1.11492368
## 8          8  8    h  0.05390516
## 9          9  9    i -0.56058983
## 10        10 10    j -0.60099024
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


#3.RODBC(this package is suitable for all databases,including mysql,sqlserver,oracle,ect)

###(1)first you should set the environment and dbname you use in this project(if you don't know how to do this,you can google it)

###(2)connect to the database that is set above


```r
con<-odbcConnect("mysql",uid="root",pwd="zhaolei19930410")
sqlTables(con)  #this function is used for showing tables in the danames above
```

```
##   TABLE_CAT TABLE_SCHEM TABLE_NAME TABLE_TYPE REMARKS
## 1   php_one                   data      TABLE        
## 2   php_one                  data1      TABLE        
## 3   php_one                  test1      TABLE        
## 4   php_one                  test2      TABLE        
## 5   php_one                  test3      TABLE        
## 6   php_one                  test4      TABLE        
## 7   php_one                  test5      TABLE        
## 8   php_one                  test6      TABLE        
## 9   php_one                  test7      TABLE
```
###(3)we write table into database(it's avaiable for chinese character)


```r
a<-c(1:3)
b<-c("zh","赵","李")
dat2<-data.frame(a=a,b=b)
sqlSave(con,dat2,tablename="test8")
sqlQuery(con,"select * from test8")
```

```
##   rownames a  b
## 1        1 1 zh
## 2        2 2 赵
## 3        3 3 李
```

```r
sqlFetch(con,"test8")
```

```
##   a  b
## 1 1 zh
## 2 2 赵
## 3 3 李
```

```r
odbcClose(con)
```







