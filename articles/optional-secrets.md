# Куда войти? Секреты Optional
[Telegram: Contact @kydavoiti](https://t.me/kydavoiti/85)

Всем привет! **Очень часто** я сталкивался и сталкиваюсь с `Optional` в своей жизни. Эти столкновения не оставляют меня равнодушным. Поэтому я решил ,что некоторые моменты стоит прояснить и запечатлеть, чтобы не повторяться в будущем.

Кто хочет прокачать понимание `Optional` — я вам рад. Материала, аналогичного этому, я в интернете не находил. **Эксклюзив**!

**Давайте бороться** с неверным использованием `Optional` **вместе**! **Распространяйте эту листовку** в своем ЖЭК! 👮

## Что такое Optional 
`Optional` — это *контейнер, который может содержать и не содержать в себе значение*.

Так говорит [документация](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Optional.html). 

Авторы языка **осмелились дать рекомендацию** по употреблению класса: 
> `Optional` is primarily intended for use as a method return type where there is a clear need to represent “no result,” and where using `null` is likely to cause errors. A variable whose type is `Optional` should never itself be `null`; it should always point to an `Optional` instance.  

Перевод на русский: 
> `Optional` призван, *в первую очередь*, **отображать отсутствие результата в типе возвращаемого значения**, где `null` может вызвать ошибку. Переменная типа `Optional` не должна быть равна `null`; она всегда должна указывать на экземпляр класса `Optional`.  

Многие не придают этому значения. Данная рекомендация игнорируется потому, что там написано *в первую очередь*, а не *всегда*.  Такое пренебрежение к советам из документации может плохо повлиять на качество вашего кода. Я покажу как. 

## Как не делать?
### Jacoco, аташол!

Пример кода: 
```java 
    public static String fromResource(AbstractFileResolvingResource resource) {
        String filename = Optional.ofNullable(resource.getFilename()).orElse("");
        return getMimeType(filename);
    }
```
[spark/MimeType.java at 54079b0f95f0076dd3c440e1255a7d449d9489f1 · perwendel/spark · GitHub](https://github.com/perwendel/spark/blob/54079b0f95f0076dd3c440e1255a7d449d9489f1/src/main/java/spark/staticfiles/MimeType.java#L109)

Красивое. Очень. Но когда **пишется такой код — ленивый программист совершает выстрел в бизнес-ногу**. Да, мы обезопасились от *NPE*, но это *NPE* могло быть вызвано отсутствием данных в `resource.getFilename()`, а это *бизнес-логика*. Является это *бизнес-логикой* потому, что применяется умолчание `""`, если значения нет. 

Причем тут *jacoco*? *Jacoco* будет удовлетворен единственным  тестом, что вызывает этот метод. Для него все ветки будут покрыты. Хотя она тут подразумевается не одна не одна. Более того, тут может быть *NPE*, если `resource == null`. Наверно следует допустить, что это значение `NonNull`.

Я позволю себе немного отрефаторить код: 
```java
    public static String fromResource(@NonNull AbstractFileResolvingResource resource) {
        String filename = resource.getFilename();
        if (resource.getFilename() == null) {
             // return getMimeType(filename);
             // Там по коду ниже видна эквивалентность замены; 
             // Хотя можно было бы и оставить вариаент выше.
             return "application/octet-stream";
        }
        return getMimeType(filename);
    }

    // Или, если любите ?:

    public static String fromResource(@NonNull AbstractFileResolvingResource resource) {
        String filename = resource.getFilename();
        return resource.getFilename() == null 
            ? "application/octet-stream" 
            : getMimeType(filename);
    }
```

В чем плюсы вариантов **без** `Optional`? В том, что *jacoco*  при проверке покрытия ветвей и при требовании к покрытию больше 50% — **попросит вас написать тест на оба случая**.  Такое использование `Optional` влечет то, что код останется не протестирован. Так можно упрятать любое кол-во бизнес решений. Поэтому, **используя Optional , вы осознанно или нет понижаете покрытие своего кода тестами**, если не строите покрытие у себя в голове, держа все случаи в уме.  Вы такой гений? Не понимаю тогда, что вы вообще делаете, читая этот текст 🤔

Примеры покрытия: 
```java
// Code 

@UtilityClass
public class Utils {
    public static String getUpperCaseIFNotBlankOrDefault(Map<String, String> map, String key, String defaultValue) {
        return Optional.ofNullable(map.get(key))
                .filter(StringUtils::isNotEmpty)
                .map(String::toUpperCase)
                .orElse(defaultValue);
    }

    public static String getUpperCaseIFNotBlankOrDefaultNoOpt(Map<String, String> map, String key, String defaultValue) {
        String value = map.get(key);
        // Тут специально не StringUtils, суть не поменяется.
        if (value == null || value.isEmpty()) { // Missed 2 of 4 branches
            return defaultValue; // Not covered
        }

        return value.toUpperCase(Locale.ROOT);
    }
}

// Tests 

public class UtilsTest {

    @Test
    public void testGetUpperCaseIFNotBlankOrDefault() {
        HashMap<String, String> map = new HashMap<>();
        map.put("key", "value");

        String upperCaseIFNotBlankOrDefault = Utils.getUpperCaseIFNotBlankOrDefault(map, "key", "default");

        assertThat(upperCaseIFNotBlankOrDefault).isEqualTo("VALUE");
    }

    @Test
    public void testGetUpperCaseIFNotBlankOrDefaultNoOpt() {
        HashMap<String, String> map = new HashMap<>();
        map.put("key", "value");

        String upperCaseIFNotBlankOrDefault = Utils.getUpperCaseIFNotBlankOrDefaultNoOpt(map, "key", "default");

        assertThat(upperCaseIFNotBlankOrDefault).isEqualTo("VALUE");
    }
}
```

Идентичные тесты покрывают код с `Optional` и не покрывают без.

### Allocation master

> Это не критичная проблема. Сборка будет в молодом  поколении. **НО!** Я предлагаю не забывать об этом.  

Каждый вызов промежуточного действия (неочень подходящее для `Optional` слово, потому что он не ленивый): `map`, `filter`, `flatMap` — создает новый экземпляр `Optional` и выполняет  проверки на `null`,  потому `Optional` — [это класс, а не интерфейс](https://github.com/AdoptOpenJDK/openjdk-jdk11/blob/master/src/java.base/share/classes/java/util/Optional.java#L64).  Вторая проблема не была актуальна, если бы было бы два типа `EmptyOptional` & `ValuableOptional`, тогда бы решение принималось непосредственно-и-только через *виртуальную таблицу функций.* 

Предупрежу вопрос о том, *почему я тогда не говорю, что стоит избегать Stream по той же причине*? Потому что `Stream` [может сделать оптимизации](https://www.baeldung.com/java-spliterator), которые вы можете сделать, но не всегда будете этим озабочены и забудете/не сможете/будет лень. Делает он их на основании характеристик источника данных потока, которые есть в методе [characteristics](https://docs.oracle.com/javase/8/docs/api/java/util/Spliterator.html#characteristics--). Он не толко налагает дополнительные расходы, но и предоставляет оптимизации. 

## Как делать?
Все сказано в документации:
> `Optional` призван, *в первую очередь*, **отображать отсутствие результата в типе возвращаемого значения**, где `null` может вызвать ошибку. Переменная типа `Optional` не должна быть равна `null`; она всегда должна указывать на экземпляр класса `Optional`.  

Если говорить примерами, то вот так: 
```java
        public Optional<String> rawCompiledVersion() {
            if (compiledVersion != null) {
                return Optional.of(compiledVersion.toString());
            } else {
                return Optional.ofNullable(rawCompiledVersion);
            }
        }
```
[Bytecoder/ModuleDescriptor.java at 8d897fc1f9b854b771d843a60d5631f4472501dc · mirkosertic/Bytecoder · GitHub](https://github.com/mirkosertic/Bytecoder/blob/8d897fc1f9b854b771d843a60d5631f4472501dc/classlib/java.base/src/main/resources/META-INF/modules/java.base/classes/java/lang/module/ModuleDescriptor.java#L241)

А не вот так: 
```java
        public String rawCompiledVersion() {
            return Optional.ofNullable(compiledVersion)
                .map(Object::toString)
                .orElse(rawCompiledVersion);
        }
```

И даже не так: 
```java
        public Optional<String> rawCompiledVersion() {
            String result = Optional.ofNullable(compiledVersion)
                .map(Object::toString)
                .orElse(rawCompiledVersion);

            return Optional.ofNullable(result);
        }
```

Хоть и очень хочется сделать такую *функциональную (но играющую против тебя) цепочку*. 

## Вывод
Я показал как не стоит  употреблять `Optional`. Употребление его в таких случаях можно назвать **экономией мысли**. Надеюсь вы подчерпнули для себя что-то новое или хотя бы задумались.

Еще напомню о моем старом посте: [код пишет код](https://github.com/IlyasYOY/IlyasYOY/blob/master/articles/code-writes-code.md). Если в проекте `Optional`  будет использовать таким способ, как я сегодня противопоказал, то можно будет встретить такой *код-звоночек*: 
```java 
  @Override
  @Nullable
  public Long getLockId(String entryId, LockType lock)
  {
    return getLocks(entryId).entrySet().stream()
                            .filter(entry -> entry.getValue().equals(lock))
                            .map(Entry::getKey)
                            .findAny()
                            .orElse(null);
  }
```
[druid/SQLMetadataStorageActionHandler.java at ad6609a606c0ca945c6aa57f1b7c40593ba29f80 · apache/druid · GitHub](https://github.com/apache/druid/blob/ad6609a606c0ca945c6aa57f1b7c40593ba29f80/server/src/main/java/org/apache/druid/metadata/SQLMetadataStorageActionHandler.java#L625)

А надо так:
```java 
  @Override
  public Optional<Long> getLockId(String entryId, LockType lock)
  {
    return getLocks(entryId).entrySet().stream()
                            .filter(entry -> entry.getValue().equals(lock))
                            .map(Entry::getKey)
                            .findAny();
  }
```

Спасибо за внимание! 
