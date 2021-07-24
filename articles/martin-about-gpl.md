# Куда войти? Про GPL
[Telegram: Contact @kydavoiti](https://t.me/kydavoiti/39)

Наткнулся на [статейку](https://martin.kleppmann.com/2021/04/14/goodbye-gpl.html) от автора книги с кабанчиком. Статья называется *It’s time to say goodbye to the GPL*. Мне показалось, что выводы, которые сделал автор неверны и ситуации не рассмотрена в полной мере. Если кому интересно прочитать про лицензии и как они влияют на *IT* — добро пожаловать. Как всегда — обратную связь можете писать мне в [ЛС](https://t.me/im_ilya), либо оставьте комментарий к посту.

Разбор будет проходить по цитатам. Цитаты на английском, переводить не буду. Курсив и жирный мой, если иное не отмечено явно.

Заранее извиняюсь, что в некоторых местах я толдычил одно и тоже. Я считаю, что без такого самоповтора локальность повествования была бы принесена в жертву, а это важнее для раскрытия, мы тут не литераторы.

**Талантливый человек — талантлив не во всем**. Даже такой эксперт в своей области может противоречить себе в одном абзаце, что я покажу ниже. Автор показал абсолютное непонимание процесса появления продуктов с идеологией открытого исходного кода и *copyleft* политикой. Автор не понял почему на смену им пришла *permissive* политика. Более того, он побуждает её использовать! Я разочарован в Мартине.

## Разбор
Я не буду обсуждать Столлмана, как и автор оригинальный статьи. 

> In this post I argue that we should move away from the GPL and related licenses (LGPL, AGPL), […] I think they have failed to achieve their purpose, and **they are more trouble than they are worth**.  

Автор говорит, что лицензии *LGPL*, *AGPL* (далее будет просто *GPL*) не достигли той цели, которой они должны были достичь и сейчас больше составляют проблемы, чем дают пользы.

Автор статьи хорошо описал суть лицензий *GPL*.

> […] the defining feature of the GPL family of licenses is the concept of  [copyleft](https://en.wikipedia.org/wiki/Copyleft) , which states (roughly) that if you take some GPL-licensed code and modify it or build upon it, you must also make your modifications/extensions (known as a “ [derivative work](https://en.wikipedia.org/wiki/Derivative_work) ”) freely available under the same license. This has the effect that the GPL’ed source code cannot be incorporated into closed-source software. At first glance, this seems like a great idea. […]  

Далее под громким заголовком **The enemy has changed** следует цитата: 

> In the 1980s and 1990s, when the GPL was written, the enemy of the free software movement was Microsoft and other companies that sold closed-source (“proprietary”) software.  

Я нигде не нашел, что целью было именно это. Если подумать логически, то сама лицензия *GPL* пытается сделать так, чтобы как можно больше программного обеспечения было распространено “открыто” (под лицензией *GPL*). Все это сделано с расчетом на лавинный эффект. Чем больше *ПО* уже есть с лицензией GPL — тем больше шансов, что новый продукт будет использовать в себе что-то свободное, а далее в силу лицензии тоже будет открыт. 

Далее я приведу 2 цитаты  и помечу их ,чтобы было проще ссылаться.

> Closed-source software cannot easily be modified by users; you can take it or leave it, but you cannot adapt it to your own needs. To counteract this, the GPL was designed to force companies to release the source code of their software, so that users of the software could study it, modify it, compile and use their modified version, and thus have the freedom to customise their computing devices to their needs.  
[1]

> Moreover, GPL was motivated by a desire for fairness: if you write some software in your spare time and release it for free, it’s understandable that you don’t want others to profit from your work without giving something back to the community. Forcing derivative works to be open source ensures at least some baseline of “giving back”.  
[2]

Сразу за этими двумя цитатами следует: 

> While this made sense in 1990, I think the world has changed, and **closed-source software is no longer the main problem**.  

Автор почему-то не удосужился рассказать что значит его главная проблема. Исходя из [1], [2] я не вижу, чтобы *GPL* боролась с коммерческим ПО. Лицензия пытается достичь *справедливости*, если быть точнее, то чтобы любая работа разработчика была достоянием общественности. Цель *make others to profit from your work without giving something back to the community*, если ты сделал эту работу достоянием общественности. По-моему эта цель далеко за пределами *closed-source software*, это цель настолько благая, что за нее боятся не только в среде разработки, но и в любой среде, где есть право на интеллектуальную собственность ([ссылочка](https://www.youtube.com/watch?v=0_PeL87Gb2s&t=10s)).

Автор сам поставил цель лицензии (борьба с *commercial software*), сам привел две причины [1] [2] почему *GPL* поломает бизнес, но нигде нет привязки к *commercial software*. Первый звоночек, что автор что-то перепутал.

Далее автор указывает, что: **In the 2020s, the enemy of freedom in computing is cloud software**. У нас скоро произойдет подмена понятий. Автор сначала сказал, что *GPL* в 90 боролась с закрытым ПО (что верно), доказал этим тем, что [1], [2] (что верно), но только потому, что цели *GPL* **намного** шире, чем цели определяемые автором перед ней. 

Автор жалуется, что раньше: 
> […] with closed-source software that runs on your own computer, at least someone could reverse-engineer the file format it uses to store its data […]  

А сейчас в суровом 2020: 
> […] With cloud software, not even that is possible, since the data is only stored in the cloud, not in files on your own computer.  

Ох, как раньше было хорошо, можно было нарушить лицензию и расширить софт даже без *GPL*, но с *GPL* было бы еще лучше. Только вот автор не понимает, видимо, что и проблема 2020 года не существовала бы для ПО, что распространяется по *GPL*. Также автор сразу говорит, что: 

> If all software was free and open source, these problems would all be solved.  

То есть он понимает это, но не может сложить `2 + 2`. Да, *GPL* не пытается добиться того, чтобы все ПО было открыто, так как эта лицензия может сделать открытым только приложения, что используют решения с лицензией *GPL*.

Далее предлагается прекрасное решение **Local-first software**. Я даже не буду стараться пояснить почему это просто инфантильная мечта умного человека. Оставлю это как задачу тем ,кому делать нечего, дискуссия открыта. 

> Cloud software, not closed-source software, is the real threat to software freedom  

Автор: “Если все продукты будут открытыми, то проблем не будет”
Автор две рюмки спустя: “Проблема не в том, что код ПО закрыт, проблема в том, что данные не локально хранятся 😡”

> Cloud software, not closed-source software, is the real threat to software freedom, because the harm from being suddenly locked out of all of your data at the whim of a cloud provider is much greater than the harm from not being able to view and modify the source code of your software. **For that reason, it is much more important and pressing that we make local-first software ubiquitous**.   

Но в открытом ПО такой проблемы не было бы, не было бы и многих других проблем. 

> we can also make more software open-source, then that would be nice, but that is less critical  

Мы можем решить эту проблему кардинально, а также решить другие проблемы, которые автор поднимал в [1], [2], но он почему-то осознанно отказывается от этого решения, говорит, что оно *less critical*.

> Focus on the biggest and most urgent challenges first.  

Проблемы того, что фотки автора хранятся на серверах *Instagram*, а не у него локально, в ходе рассуждения стала важнее проблема свободного доступа людей к идеям товарищей. Какие-то эгоистичные выводы, как мне кажется 🤔

> Copyleft software licenses are a legal tool that attempts to force more software vendors to release their source code. In particular, the  [AGPL](https://en.wikipedia.org/wiki/Affero_General_Public_License)  is an attempt to force providers of cloud services to release the source of their server-side software. However, this hasn’t really worked: most vendors of cloud software simply refuse to use AGPL-licensed software, and either use a different implementation with a more permissive license, or re-implement the necessary functionality themselves, or  [buy a commercial license](https://www.elastic.co/pricing/faq/licensing)  that comes without the copyleft clauses.  

Да, не буду спорить, что идея провалилась, но другой судьбы у неё быть и не могло. Как и все, чего касается жажды заработать. Пока есть люди, что готовы за деньги *перебивать* продукты *GPL* в их закрытые аналоги — будет эта проблема. Это слишком большой топик чтоб обсуждать тут. Если надо — обсудим потом.

> I don’t think the license has caused any source code to become available that wouldn’t have been open source anyway  

Смелое заявление, проверять я его конечно не буду. Просто скажу в ответов это: I think the license has caused any source code to become available that wouldn’t have been open source otherwise. Мое утверждение напрямую отражает политику, которую несет за собой *GPL* лицензия, а за словами автора только *I don’t think* (что он и делает на протяжении всей статьи).

> As a legal tool to promote greater software freedom, I believe copyleft software licenses have largely failed, since they have done nothing to stop the rise of cloud software, and probably not done much to increase the share of software whose source is available. Open source software has become very successful, but much of this success is in projects with non-copyleft licenses (e.g. Apache, MIT, or BSD licenses), and even in the GPL-licensed projects (e.g. Linux) I am skeptical that the copyleft aspect was really an important factor in the project’s success.  

Да, они проиграл бой, но не проиграли войну. Если бы у них было больше сторонников, то они бы развивались лучше. Не будет же кампания (Google) релизить свой продукт (K8s) под *GPL*, они не хотят открывать код **ВСЕХ** своих продуктов 🤑. А автор предложил идею (local-first), которую непонятно как реализовать, непонятно как она будет распространяться сама по себе в альтернативу более зрелой идее. Детский сад. 

Далее автор говорит, что:
> You might argue that a software license is something that an individual developer can control, whereas governmental regulation and public policy is a much bigger issue outside of any one individual’s power. Yes, but how much impact can you really have by choosing a software license? Anyone who doesn’t like your license can simply choose not to use your software, in which case your power is zero. Effective change comes from collective action on big issues, not from one person’s little open source side project choosing one license over another.  

Автор никогда не писал бизнес-решения? Сомневаюсь. Но почему-то он думает, что лорд бизнес решит проблему разработчиков лучше чем сами разработчики.

Перейдем к выводам автора: 
> The GPL and other copyleft licenses are not bad; I just think they’re pointless. They have practical problems, and they are tainted by the behaviour of the FSF, but most importantly, I do not believe they have been an effective contributor to software freedom. The only real use for copyleft nowadays is by commercial software vendors ( [MongoDB](https://www.mongodb.com/licensing/server-side-public-license/faq) ,  [Elastic](https://www.elastic.co/pricing/faq/licensing) ) who want to stop Amazon from providing their software as a service – which is fine, but it’s motivated purely by business concerns, not by software freedom.  

Бизнес додумался как использовать *GPL*, автор нет. 1:0 в пользу толстосумов.

> For all these reasons, I think it no longer makes sense to cling on to the GPL and copyleft. Let them go. Instead, **I would encourage you to adopt a permissive license for your projects**   

Из-за таких авторов *OS* *has failed*.
