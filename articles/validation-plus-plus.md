# Куда войти? Spring Validation 101++
[Telegram: Contact @kydavoiti](https://t.me/kydavoiti/43)

Привет! В ходе мой каждодневной работы было замечено странное поведение *Spring Validation*, которое мы сегодня рассмотрим подробнее. Без лишних слов сразу к делу. 

## Проблема 
Работа с проверкой вложенных структур — дело редкое. Чаще всего у меня оно встречается в классах `ConfigurationProperties`, что мы рассмотрели в [прошлой заметке](https://t.me/kydavoiti/6). Рассмотрим подробнее на [старом примере](https://github.com/IlyasYOY/sprint-boot-validation).

## Эксперименты
Будем проводить отдельно эксперименты и искать то, когда у нас *Spring* начнет проводить проверку аннотаций. Далее попробуем найти этому пояснение в документации. 

У меня есть 2 *DTO*: 

[sprint-boot-validation/CreateNewsDto.java at master · IlyasYOY/sprint-boot-validation · GitHub](https://github.com/IlyasYOY/sprint-boot-validation/blob/master/src/main/java/com/example/validation/controller/dto/CreateNewsDto.java)
```java
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
@FieldDefaults(level = AccessLevel.PRIVATE)
public class CreateNewsDto {
    @NotBlank
    String title;
    @NotBlank
    String text;
    @Valid
    @Builder.Default
    @JsonProperty("authors")
    List<@NotNull AuthorInfoDto> authorInfoDto = Collections.emptyList();
}
```
[sprint-boot-validation/AuthorInfoDto.java at master · IlyasYOY/sprint-boot-validation · GitHub](https://github.com/IlyasYOY/sprint-boot-validation/blob/master/src/main/java/com/example/validation/controller/dto/AuthorInfoDto.java)
```java
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
@FieldDefaults(level = AccessLevel.PRIVATE)
public class AuthorInfoDto {
    @NotBlank
    String name;
    @NotBlank
    @Email
    String email;
}
```

Также имеет такой запрос в соответствующую ручку: 
[sprint-boot-validation/create_news_with_author.json at master · IlyasYOY/sprint-boot-validation · GitHub](https://github.com/IlyasYOY/sprint-boot-validation/blob/master/src/test/resources/files/create_news_with_author.json)
```json
{
  "title": "Your life changed...",
  "text": "It will never be the same again.",
  "authors": [
    {
      "name": null, // должен вызвать валидацию
      "email": "johndoe@gmail.com"
    }
  ]
}
```

Также имеем вот такой тест: 
[sprint-boot-validation/ValidationApplicationTests.java at master · IlyasYOY/sprint-boot-validation · GitHub](https://github.com/IlyasYOY/sprint-boot-validation/blob/master/src/test/java/com/example/validation/ValidationApplicationTests.java)
```java
@Test
@DisplayName("Test news creation with authors")
void createWithAuthor() throws Exception {
    String file = loadFile("create_news_with_author.json");
    mockMvc.perform(post("/news")
            .contentType(MediaType.APPLICATION_JSON)
            .content(file)
    ).andDo(print())
            .andExpect(status().is4xxClientError());
}
```

То есть запускаем и проверяем ,что *Spring* работал проверку поля и отдал ошибку.

Класс контроллера и ручка, которую будем мучать: [sprint-boot-validation/NewsController.java at master · IlyasYOY/sprint-boot-validation · GitHub](https://github.com/IlyasYOY/sprint-boot-validation/blob/master/src/main/java/com/example/validation/controller/NewsController.java#L63) 

### Контроллер без Validated && Valid

Конфигурация, которая не должна отработать ни по какой логике, но проверить надо.

```java
// ...

@RestController
@RequiredArgsConstructor
@RequestMapping(NewsController.ROOT_URL)
public class NewsController {

    // ... 

    @PostMapping
    public ResponseEntity<NewsDto> save(@RequestBody CreateNewsDto createNewsDto) {
        News news = newsService.save(createNewsDto);
        NewsDto newsDto = convertNews(news);

        return ResponseEntity.ok(newsDto);
    }

    // ...
}
```

Что и требовалось доказать, Получили: 
![](validation-plus-plus/31A819BD-10B0-4C6E-8FA1-C4B8338B8A38%202.png)

### Контроллер без Validated && с Valid

```java
// ...

@RestController
@RequiredArgsConstructor
@RequestMapping(NewsController.ROOT_URL)
public class NewsController {

    // ... 

    @PostMapping
    public ResponseEntity<NewsDto> save(@Valid @RequestBody CreateNewsDto createNewsDto) {
        News news = newsService.save(createNewsDto);
        NewsDto newsDto = convertNews(news);

        return ResponseEntity.ok(newsDto);
    }

    // ...
}
```

Все проверяется.
![](validation-plus-plus/E1775A81-FE1B-459C-AAF0-399A37D140F2%203.png)

### Контроллер с Validated && с Valid

```java
// ...

@Validated
@RestController
@RequiredArgsConstructor
@RequestMapping(NewsController.ROOT_URL)
public class NewsController {

    // ... 

    @PostMapping
    public ResponseEntity<NewsDto> save(@Valid @RequestBody CreateNewsDto createNewsDto) {
        News news = newsService.save(createNewsDto);
        NewsDto newsDto = convertNews(news);

        return ResponseEntity.ok(newsDto);
    }

    // ...
}
```

Все проверяется.
![](validation-plus-plus/E1775A81-FE1B-459C-AAF0-399A37D140F2%204.png)

### Контроллер с Validated && без Valid

```java
// ...

@Validated
@RestController
@RequiredArgsConstructor
@RequestMapping(NewsController.ROOT_URL)
public class NewsController {

    // ... 

    @PostMapping
    public ResponseEntity<NewsDto> save(@RequestBody CreateNewsDto createNewsDto) {
        News news = newsService.save(createNewsDto);
        NewsDto newsDto = convertNews(news);

        return ResponseEntity.ok(newsDto);
    }

    // ...
}
```

Все проверяется.
![](validation-plus-plus/68A8E97C-03CA-4C2C-87E3-81744EED0C24%202.png)

## Вывод
Илья, у тебя биполярная, неужели так не должно быть? Да, так и должно быть, просто в первом тексте я забыл упомянуть, что `@Valid` надо ставить над внутренним полем, иначе не будет работать. В `ConfigurationProperties` не надо ставить `@Valid`, не спрашивайте почему, я окончательно запутался 😥

> Также мне казалось, что я нашел багу и неверное поведение, н оме только показалось. Я делал ошибку в тестах. Как тестировать тесты? Тесты тестов…Вот это путь к биполярке. Спасибо за внимание.   
