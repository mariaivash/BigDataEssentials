# Лабораторна робота № 1. Завантаження та зчитування даних.

## Task1
### За допомогою download.file() завантажте любий excel файл з порталу
http://data.gov.ua та зчитайте його (xls, xlsx – бінарні формати, тому
встановить mode = “wb”. 
Виведіть перші 6 строк отриманого фрейму
даних.

```r
setwd("C:\\Users\\Mary\\Downloads")
if (!file.exists("aes.xlsx")) {
  fileUrl <- "https://data.gov.ua/dataset/2cde453d-a726-40e8-95f5-03eb05d4bfcc/resource/2e477324-76a3-4802-8e80-0ec2dc196a03/download/info_aes_blocks_04_03_2019.xlsx"
  download.file(fileUrl, destfile = "aes.xlsx", mode = "wb")
  
}

install.packages(xlsx)
library(xlsx)
aesData <- read.xlsx("aes.xlsx", sheetIndex = 1, header = TRUE)
head(aesData)

   station unit_number installed_capacity
1 Р—РђР•РЎ           1               1000
2 Р—РђР•РЎ           2               1000
3 Р—РђР•РЎ           3               1000
4 Р—РђР•РЎ           4               1000
5 Р—РђР•РЎ           5               1000
6 Р—РђР•РЎ           6               1000
   reactor_type fuel
1 Р’Р’Р•Р -1000 РўР’
2 Р’Р’Р•Р -1000   Рў
3 Р’Р’Р•Р -1000 РўР’
4 Р’Р’Р•Р -1000 РўР’
5 Р’Р’Р•Р -1000 РўР’
6 Р’Р’Р•Р -1000   Рў
  СЃommercial_operation_date
1                 1985-12-25
2                 1986-02-15
3                 1987-03-05
4                 1988-04-14
5                 1989-10-27
6                 1996-10-17
  lifetime_.extension
1          2016-09-19
2          2016-10-06
3          2017-11-07
4          2018-10-11
5                <NA>
6                <NA>
```

## Task2
### За допомогою download.file() завантажте файл getdata_data_ss06hid.csv за
посиланням та завантажте дані в R.

```r
if (!file.exists("getdata_data_ss06hid.csv")) {
  fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv"
  download.file(fileUrl, destfile = "getdata_data_ss06hid.csv")
  
}

trying URL 'https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv'
Content type 'text/csv' length 4246554 bytes (4.0 MB)
downloaded 4.0 MB

csvData <- read.table("getdata_data_ss06hid.csv", sep = ",", header = TRUE)
sum(csvData$VAL == 24, na.rm = TRUE)

[1] 53
```

## Task3
### Зчитайте xml файл за посиланням. Скільки ресторанів мають zipcode 21231?

```r
install.packages("XML")
WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

https://cran.rstudio.com/bin/windows/Rtools/
Installing package into ‘C:/Users/Mary/Documents/R/win-library/3.6’
(as ‘lib’ is unspecified)
trying URL 'https://cran.rstudio.com/bin/windows/contrib/3.6/XML_3.98-1.19.zip'
Content type 'application/zip' length 4610686 bytes (4.4 MB)
downloaded 4.4 MB

package ‘XML’ successfully unpacked and MD5 sums checked

The downloaded binary packages are in
	C:\Users\Mary\AppData\Local\Temp\RtmpmQ0CRE\downloaded_packages
```
```r
fileUrl <- "http://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Frestaurants.xml"
xmlTable <- xmlTreeParse(fileUrl, useInternal = TRUE )
xmlData <- xmlRoot(xmlTable)
xmlName(xmlData)
zip <- xpathSApply(xmlData, "//zipcode", xmlValue)
sum(zip == 21231, na.rm = TRUE)

[1] 127
```
