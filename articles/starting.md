# Куда войти? Я сказал стартуем
Хочется поделиться форматом проекта для *Spring Boot Starter*. Я частенько читал их, писал их. Надеюсь мои советы будут полезными. Если понравилось, что тут написано, то больше есть в канале: [Telegram: Contact @kydavoiti](https://t.me/kydavoiti/43). Если есть вопросы ко мне, то тогда это можно задать в ЛС: [Telegram: Contact @Im_Ilya](https://t.me/Im_Ilya).

Я не вижу смысла распинаться на эту тему, поэтому постараюсь кратко все описать.

## Spring Boot Starter
Начать надо с того какие проблемы решает инструмент. Из [документации](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using.build-systems.starters): 
> Starters are a set of *convenient dependency descriptors* that you can include in your application.   

То есть *Spring Boot Starter* — это набор конфигураций, который ты можешь подключить к своему приложению. Они настраиваются, основываясь на том, что есть в *Spring Context* приложения или в *Class Path*. 

## Какие проблемы я встречал? 
Я встречал проблемы с тем, что стартер неверно разбивается на модули. То есть авторы не видят то, как части стартера могут использоваться отдельно друг от друга.

Чтобы понять как правильно это делать — надо почитать документацию и подумать. 

## Продумаем Spring Boot Starter 🤔
Для примера возьмем простую задачу. У нас есть репозиторий `solid-metrics`. Этот проект содержит классы для работы с метриками. Наша команда перешла на *Spring Boot* и нам требуется организовать стартер для этой библиотеки.

Нам точно понадобится сделать многомодульный проект. Первый модуль будет называться `solid-metrics`. Он будет содержать саму библиотеку, которая до этого была в корневом проекте репозитория.

### Какие у нас будут модули? 

- `autoconfigure`
- `starter`
Зачем нужны эти 2 модуля? 

- - - -

#### Что говорит документация? 

[Spring Boot Reference Documentation](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.developing-auto-configuration.custom-starter)

> The **autoconfigure** module that contains the auto-configuration code for “acme”.  
То есть этот модуль содержит то, что обычно называется *Spring Config* для приложения. Т.е. это помеченные `@ConfigurationProperties` & `@Configuration` классы, 

> The **starter** module that provides a dependency to the autoconfigure module as well as “acme” and any additional dependencies that are typically useful. In a nutshell, adding the starter should provide everything needed to start using that library.  
Это модуль, который с собой тянет *autoconfigure* модуль, а также `spring.factories` файл или `@Enable*` аннотации.

В документации отмечено: 
> This *separation in two modules is in no way necessary*. If “acme” has several flavors, options or optional features, then it is better to separate the auto-configuration as you can clearly express the fact some features are optional. Besides, you have the ability to craft a starter that provides an opinion about those optional dependencies. *At the same time, others can rely only on the autoconfigure module and craft their own starter with different opinions*.  

- - - -

Документация *Spring Boot* говорит ,что разделять их не всегда обязательно, с чем не согласен. Я считаю, что лучше *всегда* их разделять, чтобы более явно показывать свои намерения. Всегда может захотеться подключить `autoconfigure` без того, чтобы конфигурации сразу оказались в контексте. Делить всегда *солиднее*. 

Для массовки докинуть еще несколько модулей. Модули адаптеры: 
- `prometheus`. 
- `jmx`
Они нужны, чтобы метрики ,которые мы предоставляем в `solid-metrics` могли работать через `prometheus` и `jmx` соответственно. 

### Наименование

Надо придумать названия для наших модулей. 

- - - -

#### Что говорит документация? 

[Документация](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using.build-systems.starters) дает нам правило как называть стартеры: 
> third-party starter project called **thirdpartyproject** would typically be named **thirdpartyproject-spring-boot-starter**  

Также [документация](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.developing-auto-configuration.custom-starter) говорит: 
> As a rule of thumb, you should name a combined module after the starter. For example, assume that you are creating a starter for “acme” and that you name the auto-configure module acme-spring-boot and the starter acme-spring-boot-starter. If you only have one module that combines the two, name it acme-spring-boot-starter.  

- - - -

То есть наши названия будут выглядеть примерно вот так: 
- `solid-metrics-spring-boot`
- `solid-metrics-spring-boot-starter`
- `solid-metrics-prometheus`. 
- `solid-metrics-jmx`

Я лично более люблю название `solid-metrics-autoconfigure` вместо `solid-metrics-spring-boot`, так как оно более явно указывает на функцию.

### Дополнительно 

Документация также [рекомендует использовать](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.developing-auto-configuration.custom-starter.configuration-keys) для настройки именно `@ConfigurationProperties` . Потому что для них генерируется мета-информация. Также в документации можно почитать их мнение про модули [`autoconfigure`](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.developing-auto-configuration.custom-starter.autoconfigure-module) и [`starter`](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.developing-auto-configuration.custom-starter.starter-module). Оно почти такое как и мое.
