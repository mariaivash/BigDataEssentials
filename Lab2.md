# Лабораторна робота № 2. Формат HDF5. 

## Task1
# Завантажте файл з даними за посиланням 
```r
fileUrl = "https://dcc.ligo.org/public/0146/P1700337/001/H-H1_LOSC_C00_4_V1-1187006834-4096.hdf5"
download.file(fileUrl, destfile = "file.hdf5", mode="wb")
trying URL 'https://dcc.ligo.org/public/0146/P1700337/001/H-H1_LOSC_C00_4_V1-1187006834-4096.hdf5'
Content type 'text/plain; charset=UTF-8' length 125217658 bytes (119.4 MB)
downloaded 119.4 MB
```
## Task2
# Встановить в R пакет для роботи з HDF5 файлами. 
```r
install.packages("BiocManager")
BiocManager::install("rhdf5")
library(rhdf5)
```

## Task3
# Виведіть зміст файлу командою h5ls()
```r
h5ls("file.hdf5")
                 group            name       otype
0                    /            meta   H5I_GROUP
1                /meta     Description H5I_DATASET
2                /meta  DescriptionURL H5I_DATASET
3                /meta        Detector H5I_DATASET
4                /meta        Duration H5I_DATASET
5                /meta        GPSstart H5I_DATASET
6                /meta     Observatory H5I_DATASET
7                /meta            Type H5I_DATASET
8                /meta        UTCstart H5I_DATASET
9                    /         quality   H5I_GROUP
10            /quality          detail   H5I_GROUP
11            /quality      injections   H5I_GROUP
12 /quality/injections InjDescriptions H5I_DATASET
13 /quality/injections   InjShortnames H5I_DATASET
14 /quality/injections         Injmask H5I_DATASET
15            /quality          simple   H5I_GROUP
16     /quality/simple  DQDescriptions H5I_DATASET
17     /quality/simple    DQShortnames H5I_DATASET
18     /quality/simple          DQmask H5I_DATASET
19                   /          strain   H5I_GROUP
20             /strain          Strain H5I_DATASET
    dclass      dim
0                  
1   STRING    ( 0 )
2   STRING    ( 0 )
3   STRING    ( 0 )
4  INTEGER    ( 0 )
5  INTEGER    ( 0 )
6   STRING    ( 0 )
7   STRING    ( 0 )
8   STRING    ( 0 )
9                  
10                 
11                 
12  STRING        5
13  STRING        5
14 INTEGER     4096
15                 
16  STRING        7
17  STRING        7
18 INTEGER     4096
19                 
20   FLOAT 16777216
```


## Task4
# Зчитайте результати вимірів. Для цього зчитайте name Strain з групи strain
в змінну strain. Після зчитування не забувайте закривати файл командою
H5Close()
```r
strain <- h5read("file.hdf5", "strain/Strain")
H5close()
```

## Task5
# Також з «strain/Strain» зчитайте атрибут (функція h5readAttributes)
Xspacing в змінну st та виведіть її. Це інтервал часу між вимірами. 
```r
st <- h5readAttributes("file.hdf5", "/strain/Strain")$Xspacing
st

[1] 0.0002441406
```

## Task6
# Знайдіть час початку події та її тривалість. Для цього з групи meta зчитайте
в змінну gpsStart name GPSstart та в змінну duration name Duration.
```r
gpsStart <- h5read("file.hdf5", "meta/GPSstart")
gpsStart
[1] 1187006834

duration <- h5read("file.hdf5", "meta/Duration")
duration
[1] 4096
```

## Task7
# Знайдіть час закінчення події та збережіть його в змінну gpsEnd.
```r
gpsEnd <- duration + gpsStart
gpsEnd
[1] 1187010930
```

## Task8
# Створіть вектор з часу вимірів і збережіть у змінну myTime. Початок
послідовності – gpsStart, кінець – gpsEnd, крок – st.
```r
myTime <- seq(gpsStart, gpsEnd, st)
> head(myTime)
[1] 1187006834 1187006834 1187006834 1187006834
[5] 1187006834 1187006834
> tail(myTime)
[1] 1187010930 1187010930 1187010930 1187010930
[5] 1187010930 1187010930
```

## Task9
# Побудуємо графік тільки для першого мільйону вимірів. Для цього створіть
змінну numSamples, яка дорівнює 1000000
```r
numSamples <- 1000000
```

## Task10
# Побудуйте графік за допомогою функції plot(myTime[0:numSamples],
strain[0:numSamples], type = "l", xlab = "GPS Time (s)", ylab = "H1 Strain")
```r
plot(myTime[0:numSamples], strain[0:numSamples], type = "l", xlab = "GPS Time (s)", ylab = "H1 Strain")
```
![alt text](https://github.com/mariaivash/BigDataEssentials/blob/master/Rplot01.png)
