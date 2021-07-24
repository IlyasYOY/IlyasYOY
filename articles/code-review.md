# Куда войти? Неочевидное ревью.
[Telegram: Contact @kydavoiti](https://t.me/kydavoiti/46)

Каждый разработчик занимается *Code Review*. Я один из них. В этом тексте я разберу этот процесс и попробую составить такой механизм, который будет выполнять цели и не содержать в себе противоречий и потенциала к пробуксовке в предельных случаях. Как всегда, если есть вопросы, то пишите мне в [телеграм](https://t.me/Im_Ilya), больше контента от меня можно найти на [канале](https://t.me/kydavoiti).  В начале я попробую подушнить и делать все формально, в конце будет мой комментарий.

## Основные понятия
- **PR** — *Pull Request*/*Merge Request*.
- **P** — *participant* участник процесса *Code Review*.
- **CR** — **Code Review**.
- **RH** — *Review Helper*. Агент в процессе **CR**, который может совершать действия основываясь на мета информации о  **CR**.

## Цели Code Review
Прежде чем заниматься процессом оптимизации **CR** надо поставить цели. В сторону раскрытия этих целей будут проводиться действия по оптимизации.

1.  **Максимизация распространения общих практик**. Под этим подразумевается, что идеи, которые в процессе разработки команды будут закрепляться распространялись быстрее на всех её членов. Иначе это можно назвать как **унификация вида кода в проекте**.
2. **Полная занятость в процессе**. Каждый разработчик должен участвовать в **CR** и не должен страдать от бездействия.
3. **Устойчивость к отказу**. Если один из **P** отказывается проводить **CR** , то это не должно сказываться на том, что система начнет буксовать ,а будет сходиться к некоторой ограниченной пропускной способности без внешних воздействий.
4. **Минимизация максимума времени завершения  Code Review**. Требуется минимизировать время, которое **PR** может максимально находиться в состоянии **CR**. 

## Формальная постановка задачи
Пусть у нас в системе имеется поток **PR**. Также в системе имеется некоторое кол-во `n` **P**.  Каждый **P** может проводить ревью любого **PR**. Надо построить алгоритм проведения **CR**, который будет оптимальным с точки зрения критериев приведенных выше.

То есть в задаче имеется несколько связанных параметров: 
- `n` — кол-во **P**.
- `t` — среднее время прохождение **PR**.
- `f` — это алгоритм распределения **PR** на **CR** для **P**.
- `k` — кол-во **P**,которые должны согласовать **PR** для его закрытия.

Нам надо подобрать `f` и чтобы он был устойчив к изменениям остальных параметров.

## Алгоритмы решения задачи
Будет использоваться алгоритм вытягивания работы. То есть если любой из **P** захочет провести **CR**, то он должен обратиться к некому арбитру **PR**. Арбитр будет называться **RH** (*Review Helper*). Агент в процессе **CR**, который может совершать действия основываясь на мета информации о  **CR**. Только такой способ верный так как ,если работа будет ставить на исполнителей, то это будет за собой тянуть нужду в *микроменеджменте* выполнения поставленных задач. Отсутствие управления в системе, что построена не на вытягивании будет требовать усложнение алгоритма распределения и слежения за выполнением задач, так же требовать наличия алгоритма обработки невыполнения задач. Следовательно задача сводится к задаче выбора метода упорядочивания потока **PR**. Скорость можно будет регулировать параметром `k`, если считать параметр `n` постоянным.

Определим f как функцию по поиску приоритету **PR**. Чем выше приоритет **PR** — тем раньше инициируется **CR**. Ниже будут указаны способы определения величины приоритета.

### Случайное распределение
Пусть каждый **P** смотрит **PR** случайным образом. Тогда будут выполняться все условия, кроме 4. Данный способ будет стабилизировать *среднее время прохождения* **PR** (по закону [больших чисел](https://ru.wikipedia.org/wiki/%D0%97%D0%B0%D0%BA%D0%BE%D0%BD_%D0%B1%D0%BE%D0%BB%D1%8C%D1%88%D0%B8%D1%85_%D1%87%D0%B8%D1%81%D0%B5%D0%BB)), а *не минимизировать максимальное время*. 

### Чем больше делает Code Review автор
Способ звучит справедливо, но не всегда то, что звучит *справедливо* — работает оптимально. Данные алгоритм будет минимизировать минимальное время прохождения **PR**. Потому что тот, кто больше всего смотрит **PR**, скорее всего пишет их удовлетворительно для большинства. То есть у нас не выполняется пункт 4. Также у нас будет максимизировать максимальное время **CR**. Потому что те, кому сложно закончить **PR** — будут в конце списка и смотреть их будут мало. То есть у нас будет получаться то, что система разобьется на 2 части: *маргинальная* и *продвинутая*, чего мы не хотим.

### Чем недавнее был открыт Pull Request
Тут мы будем иметь почти такую же ситуацию как и в пролом пункте, только она будет менее *справедливой*. И минусы будет теже, можете сами попробовать подумать.

### Чем сложнее Pull Request
В первую очередь стоит смотреть сложные **PR**. Сложный — это:
- Много изменений
- Автор редко участвует в процессе **CR**
- **PR** находится на **CR** много времени

Хочется сказать:
> Почему это будет оптимально, если мы будет награждать асоциальное поведение?   

Но эта система оптимальна по пунктам, которые являются целью. Если кто-то отказывается проводить **CR**, то его код будет просмотрен большим кол-вом участников, что вытягивают себе **PR**, то есть он будет подвергаться большей критике и следовательно плохой код не пройдет и ему будут явно прививаться общие практики, в большой степени. Будут усиливаться слабые места. Те, кто не проводят **CR** будут далее давать более полезные советы в силу того, что их будут хорошо критиковать. Кто-то может сказать ,что люди просто перестанут делать **CR**, чтобы их быстрее смотрели. Но быстрее их смотреть не станут — их просто перестанут смотреть вовсе и люди опять начнут смотреть **PR**.  Если все перестанут смотреть **PR** просто так — тогда все встанет, но это не аргумент, потому что он может быть тогда применен к любому пункту выше. По статистике большинство людей активно участвует в процессе **CR**, если у них есть возможность, мы просто их направим в нужном русло.  В этой системе в моменте вырастет среднее время обработки **PR**, так как первыми будут смотреть сложные, а не легкие задачи. То есть в системе будет увеличиваться время прохождения простых **PR**, а уменьшаться время прохождения больших **PR**. Она будет работать в оптимальном режиме и сама себя к нему приводить. Также она будет в перспективе уменьшать среднее время прохождения **PR** при низкой текучке кадров. Система будет прозрачной в смысле, что все простые статистики (самые долгие **CR**, меньше всего участия в **CR**) показательны и теоретически идентичны. 

## Вывод
Самые **очевидные решения не всегда самые оптимальные**. Кратко и просто. Пишите комментарии, интересно будет почитать.