## Функциональные требования

* Создание поста с геолокацией и фотографиями
* Установка оценки чужого поста
    * Предпологается установка лайка. Установка дизлайка не предпорлагается.
* Создание коментария к посту
* Получение списка постов в обратном хронологическом порядке
    * Пользователя
    * Своих постов
    * Подписок
    * Постов конкретного пользователя
* Получение списка популярных мест основанного на оценках.
* Получение списка коментариев по каждому посту
* Получение оценки по каждому посту, кол-во лайков
* Подписка на пользователя

## Нефункционаольные требования

* DAU = 10 000 000
* SubscribersMax = 1000
* PostCreateDaily = 4 (can visit up to 4 places a day)
* PostMaxImagesCount = 10
* PostReadDaily = 100
* PostLikeDaily = PostReadDaily = 100
* PostCommentDaily = 50
* PostReadDaily = 50
* Сезонность присутствует, X2 в сезон.
* PageSize = 10

## Оценка нагрузки
### Transaction

```
Post {
    id // 6b
    user_id // 6b
    message // 180b
    location // 12b
    photo_ids // [6b] * amountOfPhoto
} ~ 300b
```

```
Like {
    post_id \\6b
    count \\8b
} ~ 20b
```

```
Comment {
    id //6b
    user_id //6b
    post_id // 6b
    message // 180b
} ~ 200b
```

```
image {
    post_id // 6b
    image_id // 6b
    data //2mb
} ~ 2mb
```

### RPS
```
RPS(write post) = 10 000 00(DAU) * 4 (PostCreateDaily) / 86400 ~ 50
RPS(write image) = 50 (RPS write post) * 10 (PostMaxImagesCount) = 500
RPS(write comment) = 10 000 00(DAU) * 50(PostCreateDaily) / 86400 ~ 500
RPS(write like) = 10 000 00(DAU) * 100(PostLikeDaily) / 86400 ~ 1000

RPS(read post) = 10 000 00 (DAU) * 100 (PostReadDaily) / 86400  ~ 1000
RPS(read image) = 1000 (RPS read post) * 10 (PostMaxImagesCount) ~ 10000
RPS(read comments) = 1000 (RPS read post) * 50(ComentsReadDaily) ~ 50000
RPS(read likes) = RPS (read posts) ~ 1000
```

### Traffic

```
Traffic (write posts) =  300b(write post) * 50 (RPS) ~ 150KB/s
Traffic (write images) = 2mb(image) * 10(PostMaxImagesCount) 50 (RPS write post) ~ 1GB/s
Traffic (write comments) = 200b * 500 (RPS write comments) ~ 100Kb/s
Traffic (write likes) = 20b * 1000 (RPS write likes) = 20Kb/s

Traffic (read posts) = 1000 (RPS) * 300b(post) * 10(pageSize) = 3MB/s
Traffic (read images) = 2mb(image) * 10(PostMaxImagesCount) * 10(pageSize)  ~ 10Gb/s
Traffic (read coments) = 200b * 50000 RPS(read coments) = 10Mb/s
Traffic (read likes) = 20b * 1000 RPS(read likes) = 20KB/s
```
