# Монолог про ООП
[Telegram: Contact @kydavoiti](https://t.me/kydavoiti/76)

Разговоры про **ООП** у меня встречаются *слишком часто*. Реплики стабилизировались, поэтому я решил описать их в тексте. Могу порекомендовать почитать по этому вопросу [Чистая архитектура](https://www.litres.ru/robert-s-martin/chistaya-arhitektura-iskusstvo-razrabotki-program-39113892/?utm_source=google&utm_medium=cpc&utm_campaign=search_dsa_ohvat_f%7C2087774395&utm_term=&utm_content=375733693660%7Bphrase_id%7D_%7Bsource%7D_%7Bsource_type%7D_%7Bregion_name%7D_9047064&param_2=987239&gclid=Cj0KCQjwpf2IBhDkARIsAGVo0D3iz5U9HoTCFhrMgtmXFaXkEIOYvc6YWfqWBqqWQdzrgO72oSuZ3QwaAvvaEALw_wcB) и статьи. Со вторым быть аккуратнее. 

Долго не будем мусолить — начнем.

## Понятие ООП

**ООП** — это объектно-ориентированное программирование. Это понятие можно внести кучей способов. Робер Мартин это тоже подчеркивает в [своей статье](https://blog.cleancoder.com/uncle-bob/2018/04/13/FPvsOO.html). Сам он формулирует его так: 

> The technique of using *dynamic polymorphism* to call functions *without the source code of the caller depending upon the source code of the callee*. (Выделение мое)  

Он выделяет один принцип как основной — **полиморфизм**. Остальные ему не так важны. Подробнее можно почитать по ссылке выше.

Обычно выделяют три принципа: 
- **Инкапсуляция**
- **Наследование**
- **Полиморфизм**
- ~Абстракция~ (не всегда, я отношусь к не всегда)

Но не всегда это делают верно. Вот [тут](https://training.epam.ua/#!/News/275?lang=ru), например. Часто все понятия с собой не связаны. Так быть не должно.

- - - -

**Инкапсуляция** — этот принцип говорит, что *функции по обработке данных объекта должны быть методами объекта*. 

Из этого прямо следует, что **объект** — *это не просто набор данных, но и методы для работы с ними*. **Инкапсуляция** не делать код **ОО**. Она позволяет коду быть реализованным так, чтобы *отдавать методы-как-контракт, а не данные-как-контракт*. Часто его путают с сокрытием, [это](https://training.epam.ua/#!/News/275?lang=ru) не верно. **Сокрытие деталей** — это свойство, которое **инкапсуляция** (и не только она) привносит.

```kotlin
data class Point(
  val x: Float,
  val y: Float,
) {
  // 1
  fun radius(): Float = hypot(x, y)
}

// 2
fun radius(point: Point) = hypot(point.x, point.y)
```

В случае 1 функция инкапсулирована в объект. В случае 2 она инкапсулирована в *Kotlin Script* (то есть *Java Package*).

- - - -

**Наследование** — это *способность классов перенять инкапулированные в классы-родители данные и логику*. 

Тут можно без примеров. Самый понятный принципов. Но он явно связан с прошлым понятием. *Чтобы получать больше от наследования — надо применять инкапсуляцию*.

- - - -

**Полиморфизм** — это способность объекта *переопределять поведение инкапсулированное в наследуемый им класс*.

Из определения видно, что под **полиморфизмом** я подразумеваю только один его тип — *динамический*. Другие (статический, параметрический, может еще какие есть) не являются необходимыми для **ООП**. Полиморфизм уменьшает связанность кода.  Он явно полагается на прошлые определения.

- - - -

Чтобы код был **ОО**, он должен соблюдать все принципы выше. Достаточно сказать *максимизировать полиморфность*. То есть надо пользоваться **полиморфизмом**, а чтобы делать это надо **инкапсулировать** и **наследовать**.
