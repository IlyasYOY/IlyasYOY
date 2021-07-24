# Куда войти? Spring Validation 101
[Telegram: Contact @kydavoiti](https://t.me/kydavoiti/6)

## Мотивация
Текст не появляется из ниоткуда. Процесс  *Code Review* и *mentoring* часто отсылали к этому вопросу. *Validation API* появлялся в моей жизни все чаще: вопрос это на *Review*, обсуждение с друзьями, вспомнить самому. Здесь раскрываются некоторые положения, это позволит  не возвращаться к этом вопросу снова, а отправлять ссылку на статью. 

В тексте используются части кода из [репозитория](https://github.com/IlyasYOY/sprint-boot-validation). Это приложение для работы с новостями. Код содержит  аннотации *Lombok*, надеюсь эта библиотека знакома читателю, если нет, то время [познакомиться](https://projectlombok.org). После можно будет самому себе скачать проект и поэкспериментировать.

## Spring Validation. Зачем нам это?
*Spring Validation* позволяет вынести за рамки класса часть кода, которая проверяет данные.  *Spring* предоставляет аннотации для работы с этим *API* . Аннотации я делю на 2 вида: **маркерные** и **активационные**.  **Маркерные**: `@NotBlank`, `@NotEmpty`, `@Positive`, `@Min`, `@Max`, `@PositiveOrZero`  -  аннотации показывают *Spring* как обработать значение, указывают правила проверки. **Активационные**: `@Valid`, `@Validated`- говорят *Spring* активировать проверки для элемента.
 
## @Valid, @Validated
Эти аннотации часто путают, поэтому предлагаю обратить внимание на их различия, какую где использовать. Документация - первое место, куда стоит обратиться.

### @Valid
[JDoc](https://docs.oracle.com/javaee/7/api/javax/validation/Valid.html?is-external=true)
> Marks a property, method parameter or method return type for validation cascading.   
> Constraints defined on the object and its properties are be validated when the property, method parameter or method return type is validated.  
> This behavior is applied recursively.  

Эта аннотация применяется не к классу, а к единицам информации: *поля класса*, *параметры метода*, *возвращаемое значение метода*. Поведение распространяется рекурсивно. Из документации прояснилось, что аннотация предназначается для состояния и контракта классов, а не для них самих.

В документации не указывается [поведение ](https://docs.spring.io/spring/docs/4.1.x/spring-framework-reference/html/validation.html#validation-mvc-triggering.), специфичное для *Spring*. `@Controller` по-умолчанию обрабатывает аннотаций `@Valid`.

### @Validated
[JDoc](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/validation/annotation/Validated.html)
> Variant of JSR-303’s  [Valid](https://docs.oracle.com/javaee/7/api/javax/validation/Valid.html?is-external=true) , supporting the specification of validation groups. Designed for convenient use with Spring’s JSR-303 support but not JSR-303 specific.   
> Can be used e.g. with Spring MVC handler methods arguments. Supported through  [SmartValidator](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/validation/SmartValidator.html) ’s validation hint concept, with validation group classes acting as hint objects.   
> Can also be used with method level validation, indicating that a specific class is supposed to be validated at the method level (acting as a pointcut for the corresponding validation interceptor), but also optionally specifying the validation groups for method-level validation in the annotated class. Applying this annotation at the method level allows for overriding the validation groups for a specific method but does not serve as a pointcut; a class-level annotation is nevertheless necessary to trigger method validation for a specific bean to begin with. Can also be used as a meta-annotation on a custom stereotype annotation or a custom group-specific validated annotation.  

`@Validated` является точкой применения аспекта, который проверяет и обрабатывает аннотацию `@Valid`. 

## Пользовательские проверки
*Spring* предоставляет возможность создавать маркерные аннотации. Это рассмотрим на примере.  Проверка на предмет того, что поле содержит *email* только требуемого домена. *Email* будет на домене google.com.

Для этого создадим `ConfigurationProperties`:

```java
@Getter
@Validated
@ConstructorBinding
@AllArgsConstructor
@ConfigurationProperties("email.domains")
public class EmailDomainsProperties {
    @NotNull
    private List<String> allowed;
}
```

Аннотация `@Validated` над классом гарантирует то, что проверка будет вызвана на полях.

Поменяем конфигурацию, добавим туда домены:

```yaml
email:
  domains:
    allowed:
      - google.com
```

Создадим аннотацию `@EmailDomains`:

```java
@Documented
@Constraint(validatedBy = EmailDomainsValidator.class)
@Target({METHOD, FIELD, ANNOTATION_TYPE, CONSTRUCTOR, PARAMETER})
@Retention(RUNTIME)
public @interface EmailDomains {
    String message() default "{com.example.validation.configuration.annotation.EmailDomains.message}";

    Class<?>[] groups() default { };

    Class<? extends Payload>[] payload() default { };
}
```

Данная аннотация должна содержать 3 свойства: 
- `message`: содержит текст ошибки при проверке. Пример ссылается на свойство из контекста. Подробнее можно посмотреть [здесь](https://www.baeldung.com/spring-validation-message-interpolation).
- `group`
- `payload `
Последних 2 свойства описывается в [документации](https://docs.oracle.com/javaee/7/api/javax/validation/Constraint.html), они не интересуют нас в простейшем случае. 
Важно обратить внимание на аннотацию `@Constraint(validatedBy = EmailDomainsValidator.class)`. Который указывает класс обработчик аннотации.

```java
@RequiredArgsConstructor
public class EmailDomainsValidator implements ConstraintValidator<EmailDomains, String> {

    public static final String AT_SYMBOL = "@";

    private final EmailDomainsProperties emailDomainsProperties;

    @Override
    public void initialize(EmailDomains constraint) {
    }

    @Override
    public boolean isValid(String obj, ConstraintValidatorContext context) {
        return emailDomainsProperties.getAllowed()
                .stream()
                .map(String::toLowerCase)
                .map(it -> AT_SYMBOL + it)
                .anyMatch(obj.toLowerCase()::endsWith);
    }
}
```

*Spring* может внедрять в обработчик зависимости. `initialize` отвечая за настройку объекта перед использованием, например получение информации из аннотации. `isValid` содержит логику проверки.

```java
@Getter
@Validated
@ConstructorBinding
@AllArgsConstructor
@ConfigurationProperties("defaults")
public class DefaultsProperties {
    @NotNull
    private Author author;

    @Getter
    @AllArgsConstructor
    public static class Author {
        @NotBlank
        private String name;
        @NotBlank @Email @EmailDomains
        private String email;
    }
}
```

В примере мы пометили поле для проверки . Посмотрим как это будет работать.

```yaml
defaults:
  author:
    name: Ilia Ilinykh
    email: Ilia@Ili.com

email:
  domains:
    allowed:
      - Ilia.com
```

Запуск программы показываем нам такую ошибку:

```
Field error in object 'defaults.author' on field 'email': rejected value [Ilia@Ili.com]; codes [EmailDomains.defaults.author.email,EmailDomains.email,EmailDomains.java.lang.String,EmailDomains]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [defaults.author.email,email]; arguments []; default message [email]]; default message [{com.example.validation.emaildomains.message}]; origin class path resource [application-test.yml]:4:12
```

Проверка прошла неуспешно, так как конфигурация не соответствовала требованиям.

## Примеры
Основные место для применения *Spring Validation*: проверка параметров *beans*, контроллеров, проверка значений `ConfigurationProperties`. 

### Проверка параметров методов элементов контекста
Самым главным местом применения этого *API*  является проверка входных параметров контроллеров. 

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
    @Builder.Default
    @JsonProperty("authors")
    List<@NotNull AuthorInfoDto> authorInfoDto = Collections.emptyList();
}
```

После чего можно включить проверку этих параметров на уровне контроллера. 

```java
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

Аннотация `@Valid` включает валидация на параметре. **NOTE!** Над классом не требуется указывать `@Validated`, чтобы это работало для `@Controller` классов. Чтобы валидация работала для других элементов контекста, добавьте над ним аннотацию `@Validated`. Но чтобы работали аннотации `@NotNull`, `@NotBlank` и другие, надо **ОБЯЗАТЕЛЬНО** ставить аннотацию `@Validated`. Также важно, что *Spring MVC* по-умолчанию обработает проверки, которые не требуют `@Validated` на `@Controller` вот так: 

```json
{
    "error": "Bad Request",
    "message": "",
    "path": "/news",
    "status": 400,
    "timestamp": "2020-06-28T10:33:06.109+00:00"
}
```

Ошибка обрабатывается, как 400. `@Validated` генерирует ошибки, что обрабатываются вот так: 

```json
// Тут у меня стояла проверка, что размер ID меньше 2
{
    "error": "Internal Server Error",
    "message": "",
    "path": "/news/123",
    "status": 500,
    "timestamp": "2020-06-28T10:33:12.686+00:00"
}
```

*Spring* обработал ошибку, как внутреннюю. Что не корректное поведение. Не забывайте прописывать обработчики для исключений `javax.validation.ConstraintViolationException`. 

```java
@Service
@Validated
@RequiredArgsConstructor
public class CrudNewsService implements NewsService {
	  // ...

    @Override
    public News save(@Valid CreateNewsDto createNewsDto) {
        News news = conversionService.convert(createNewsDto, News.class)
                .applyDefaults(authorDefaults, defaultsAuthorFactory);

        NewsModel newsModel = conversionService.convert(news, NewsModel.class);
        NewsModel savedModel = newsRepository.save(newsModel);
        return convertNewsModel(savedModel);
    }

	  // ...
}
```

Пример показывает, что помечать аннотациями можно не сам класс, а `interface`, который реализуют классы.

```java
@Validated
public interface NewsService {
    Optional<News> findById(String id);

    List<News> findAll();

    News save(@Valid CreateNewsDto createNewsDto);

    Optional<News> delete(String id);
}
```

**NOTE!** Это работает через *Spring AoP*, что несет за собой позитивные и негативные моменты этого. В моем случае *Spring* использовал *CGLIB*. При этом важно заметить, что аннотация `@Validated` говорит `Spring` создать прокси вокруг класса, поэтому без неё ничего не работают другие аннотации.
![](validation/CBD7CD41-FADF-4AFA-8144-6C4AD7F86BF2.png)

### Проверка ConfigurationProperties
Требования к классам такие же, как в случае других проверок.  Классы помечаются как `@Validated`. Далее *Spring* по методам `get`/`set` поймет какая требуется проверка класса.

```java
@Getter
@Validated
@ConstructorBinding
@AllArgsConstructor
@ConfigurationProperties("defaults")
public class DefaultsProperties {
    @NotNull
    private Author author;

    @Getter
    @AllArgsConstructor
    public static class Author {
        @NotBlank
        private String name;
        @NotBlank @Email @EmailDomains
        private String email;
    }
}
```

Применяйте аннотации `@AssertTrue`/`@AssertFalse`. Они делают проверки без усложнения кода добавлением аннотаций и обработчиков.

```java
@Getter
@Validated
@ConstructorBinding
@AllArgsConstructor
@ConfigurationProperties("email.domains")
public class EmailDomainsProperties {
    @NotNull
    private Set<String> allowed;

    @AssertTrue
    boolean allDomainsContainDot() {
        return allowed.stream()
                .anyMatch(s -> !s.contains("."));
    }
}
```

Пример выше проверяет, что используются только строки с точками.

## Заключение
В тексте рассмотрены моменты использования *Spring Validation API*, которые часто встречались. Хочется верить , что это поможет вам наконец-то перестать сомневаться в том, какие аннотации поставить, чтобы код работал. Так как этот *API* вызывает вопросы, а не предоставляет ответы, пока в него не погрузиться.
