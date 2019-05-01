# Лабораторна робота № 3. Зчитування даних з WEB.

```r
install.packages('rvest')
library(rvest)
```
```r
web = read_html("http://www.imdb.com/search/title?count=100&release_date=2017,2017&title_type=feature")
rank_data <- html_text(html_nodes(web,'.text-primary'))
rank_data = as.numeric(rank_data)
title_data <- html_text(html_nodes(web,'.lister-item-header a'))
runtime_data <- html_text(html_nodes(web,'.text-muted .runtime'))
runtime_data = as.numeric(gsub(" min", "", runtime_data))
movies <- data.frame(Rank = rank_data, Title = title_data, Runtime = runtime_data, stringsAsFactors = FALSE)
```
## Task1
### Виведіть перші 6 назв фільмів дата фрейму.
```r
head(movies$Title)
[1] "Тор: Рагнарок"                  
[2] "Людина-павук: Повернення додому"
[3] "Вартовi Галактики 2"            
[4] "Unicorn Store"                  
[5] "1+1: Нова iсторiя"              
[6] "Найвеличнiший шоумен"
```
## Task2
### Виведіть всі назви фільмів с тривалістю більше 120 хв.
```r
movies$Title[movies$Runtime > 120]
 [1] "Тор: Рагнарок"                           
 [2] "Людина-павук: Повернення додому"         
 [3] "Вартовi Галактики 2"                     
 [4] "1+1: Нова iсторiя"                       
 [5] "Диво-Жiнка"                              
 [6] "Зорянi вiйни: Епiзод 8 - Останнi Джедаi" 
 [7] "Воно"                                    
 [8] "Той, хто бiжить по лезу 2049"            
 [9] "Пiрати Карибського моря: Помста Салазара"
[10] "Форма води"                              
[11] "Call Me by Your Name"                    
[12] "Джон Уiк 2"                              
[13] "Чужий: Заповiт"                          
[14] "Красуня i Чудовисько"                    
[15] "Логан: Росомаха"                         
[16] "Король Артур: Легенда меча"              
[17] "Форсаж 8"                                
[18] "Мати!"                                   
[19] "Трансформери: Останнiй лицар"            
[20] "The Shack"                               
[21] "Kingsman: Золоте кiльце"                 
[22] "Гра Моллi"                               
[23] "Валерiан i мiсто тисячi планет"          
[24] "Пострiл в безодню"                       
[25] "Метелик"                                 
[26] "Примарна нитка"                          
[27] "Вороги"                                  
[28] "Brawl in Cell Block 99"                  
[29] "Вбивство священного оленя"               
[30] "Only the Brave"                          
[31] "Темнi часи"                              
[32] "Saban's Могутнi рейнджери"               
[33] "Сiм сестер"                              
[34] "Усi грошi свiту"                         
[35] "The Glass Castle"                        
[36] "Вiйна за планету мавп"
[37] "Дружина доглядача зоопарку"
```

## Task3
### Скільки фільмів мають тривалість менше 100 хв.
```r
sum(movies$Runtime < 100)
[1] 15
```
