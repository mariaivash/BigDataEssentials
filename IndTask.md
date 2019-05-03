# Індивідуальне завдання

## Аналіз набору даних щодо кількості користувачів мережі Інтернет з огляду на збільшення кількості мобільних телефонів. 

Для аналізу було використано датасет GapMinder Internet accesses databases (https://www.kaggle.com/tsilveira/gapminder-internet-accesses-databases/).
Цей набір даних містить три файли з (i) користувачами широкосмугового доступу; (ii) користувачів мобільних телефонів; та (iii) доступ до Інтернету країнами. Файли знаходяться у форматі CSV, щоб полегшити аналіз даних.

Метою дослідження стало питання використання сучасних технологій у пострадянських країнах (https://en.wikipedia.org/wiki/Post-Soviet_states), зокрема впливу мобільних телефонів на користування мережею Інтернет.

### Хід роботи:

1. Налаштуємо необхідні для аналізу інтсрументию Встановлюємо пакет *tidyverse*, який дозволить проводити різні маніпуляції над даними за допомогою функцій. Оберемо відповідну бібліотеку.
```r
setwd("C:\\Users\\Mary\\Downloads")

install.packages("tidyverse")
library(dplyr)
```

2. Зчитуємо .csv файли для подальшої обробки, присвоюємо назви змінним. Аналізуємо дві таблиці - користувачі Інтернету та користувачі мобілних телефонів.
```r
internet <- read.csv('./databases/Internet_user_total.csv', header = TRUE, row.names = 1, check.names = FALSE)
head(internet)

               1990 1991 1992 1993 1994 1995 1996 1997 1998  1999   2000   2001   2002   2003    2004    2005
Afghanistan       0   NA   NA   NA   NA   NA   NA   NA   NA    NA     NA   1118   1124  22569   28244  338045
Albania           0   NA   NA   NA   NA  351 1002 1502 2002  2502   3505  10026  12053  30194   75634  189887
Algeria           0   NA   NA   NA  100  500  500 3003 6006 60055 150137 200180 500441 700615 1501387 1921982
American Samoa    0   NA   NA   NA   NA   NA   NA   NA   NA    NA     NA     NA     NA     NA      NA      NA
Andorra           0   NA   NA   NA   NA   NA  996 1980 4424  4886   6812     NA   7775   9781   20207   29290
Angola            0   NA   NA   NA   NA   NA   97  726 2424  9723  14629  19570  40260  57159   74173  188530
                  2006    2007    2008    2009    2010 2011
Afghanistan     598865  553771  549056 1085510 1256470   NA
Albania         303350  476594  759081 1315402 1441928   NA
Algeria        2462986 3204578 3504773 3924904 4433526   NA
American Samoa      NA      NA      NA      NA      NA   NA
Andorra          39088   57681   57837   65712   68740   NA
Angola          324498  560812  829746 1113307 1908191   NA
```
```r
phone <- read.csv('./databases/cellphone_total.csv', header = TRUE, row.names = 1, check.names = FALSE)
head(phone)

  1965 1966 1967 1968 1969 1970 1971 1972 1973 1974 1975 1976 1977 1978 1979 1980 1981 1982 1983
Abkhazia                NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA
Afghanistan              0   NA   NA   NA   NA    0   NA   NA   NA   NA    0    0    0    0    0    0    0    0    0
Akrotiri and Dhekelia   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA
Albania                  0   NA   NA   NA   NA    0   NA   NA   NA   NA    0    0    0    0    0    0    0    0    0
Algeria                  0   NA   NA   NA   NA    0   NA   NA   NA   NA    0    0    0    0    0    0    0    0    0
American Samoa           0   NA   NA   NA   NA    0   NA   NA   NA   NA    0    0    0    0    0    0    0    0    0
                      1984 1985 1986 1987 1988 1989 1990 1991 1992 1993 1994 1995  1996  1997  1998  1999  2000
Abkhazia                NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA    NA    NA    NA    NA    NA
Afghanistan              0    0    0    0    0    0    0    0    0    0    0    0     0     0     0     0     0
Akrotiri and Dhekelia   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA   NA    NA    NA    NA    NA    NA
Albania                  0    0    0    0    0    0    0    0    0    0    0    0  2300  3300  5600 11008 29791
Algeria                  0    0    0    0    0    0  470 4781 4781 4781 1348 4691 11700 17400 18000 72000 86000
American Samoa           0    0    0   NA   NA   NA    0    0  700  900 1200 1250  1300  1400  1500  1800  1992
                        2001   2002    2003    2004     2005     2006     2007     2008     2009     2010     2011
Abkhazia                  NA     NA      NA      NA       NA       NA       NA       NA       NA       NA       NA
Afghanistan                0  25000  200000  600000  1200000  2520366  4668096  7898909 10500000 13000000 17558265
Akrotiri and Dhekelia     NA     NA      NA      NA       NA       NA       NA       NA       NA       NA       NA
Albania               392650 851000 1100000 1259590  1530244  1909885  2322436  1859632  2463741  2692372  3100000
Algeria               100000 450244 1446927 4882414 13661355 20997954 27562721 27031472 32729824 32780165 35615926
American Samoa          2156   2036    2100    2250       NA       NA       NA       NA       NA       NA       NA
> 
```
3. Країни вносяться як назви рядків. Для подальшої обрабки даних необхідно створити колонку з назвами країн.
```r
colnames(internet)
 [1] "1990" "1991" "1992" "1993" "1994" "1995" "1996" "1997" "1998" "1999" "2000" "2001" "2002" "2003" "2004" "2005"
[17] "2006" "2007" "2008" "2009" "2010" "2011"

colnames(phone)
 [1] "1965" "1966" "1967" "1968" "1969" "1970" "1971" "1972" "1973" "1974" "1975" "1976" "1977" "1978" "1979" "1980"
[17] "1981" "1982" "1983" "1984" "1985" "1986" "1987" "1988" "1989" "1990" "1991" "1992" "1993" "1994" "1995" "1996"
[33] "1997" "1998" "1999" "2000" "2001" "2002" "2003" "2004" "2005" "2006" "2007" "2008" "2009" "2010" "2011"
```
```r
internet$country <- rownames(internet)
phone$country <- rownames(phone)
```
4. Необхідно відфільтрувати необхідні дані. Створимо вектори з переліком потрібних значень. Для країн обираємо пострадянські держави, для часового проміжку - період між розпадом СРСР у 1991 та фінансовою кризою 2008.  
```r
countries <- c('Armenia', 'Azerbaijan', 'Belarus', 'Estonia', 'Georgia', 'Kazakhstan', 'Kyrgyzstan', 'Latvia', 'Lithuania', 'Moldova', 'Russia', 'Tajikistan', 'Turkmenistan', 'Ukraine', 'Uzbekistan')
years <- as.character(c(1991:2008))
```

5. З допомогою бібліотеки *dplyr* можемо використати функцію *select*, щоб відсортувати необхідні дані за колонками років.
```r
internet <- select(internet, c(years, 'country'))
phone <- select(phone, c(years,'country'))
```
5. Використовуючи функцію *filter* відбираємо за назвами країн - рядками.
```r
internet <- filter(internet, country %in% countries)
phone <- filter(phone, country %in% countries)
```

6. Приберемо допоміжну колонку країн, повернувши її значення назад першій колонці, та після цього видалимо останню колонку.
```r
ncol(internet)
[1] 19
ncol(phone)
[1]19

rownames(internet) <- internet[,19]
rownames(phone) <- phone[,19]

internet[,19] <- NULL
phone[,19] <- NULL
```

7. Побудуємо графік, який продемонструє кореляцію між кількістю користувачів Інтернетом і користувачів мобільних телефонів.
Для того, щоб вивести обидва графіки на одне зображення, використаємо функцію *lines* разом з функцією *plot*.
На осі x відобразимо рядки, а на осі y суми користувачів в усіх країнах за рік. NA значення пропускаємо у розрахунках.
```r
x <- colnames(internet)
y1 <- colSums(internet, na.rm=TRUE)* 10^{-6}
y2 <- colSums(phone, na.rm=TRUE)* 10^{-6}

plot(x, y1, main = "Amount of Internet and cellphone users per year", sub = "Internet is in red, phones are in green", xlab = "Years", ylab =  "Amount, mln",  type="l", col="red")
lines(x, y2, col="green")
```
### Висновок:
На графіках з результатом зеленим кольором позначено кількість користувачів мобільних телефонів, а червоним - користувачів Інтернету. До 2000 років і Інтернет, і мобільний зв'язок був розкішшю. Ми можемо спостерігати стрімке експоненціальне зростання після 2000 року. Пік епохи мобільного з'язку стався раніше, ніж пік епохи Інтернету. Дещо пізніший розвиток Інтернету, зокрема, був зумовлений також появою смартфонів, що значно спростило вихід користувачів у мережу.  
![alt text](https://github.com/mariaivash/BigDataEssentials/blob/master/InternetPhone.png)
