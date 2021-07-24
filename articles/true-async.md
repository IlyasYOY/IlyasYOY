# Куда войти? Честная асинхронность
[Telegram: Contact @kydavoiti](https://t.me/kydavoiti/35)

Кстати, иногда балдею я от *OpenSource* 🧑‍💻 и питона 🐍
Есть библиотека:  [aiofiles](https://github.com/mosquito/aiofile) . В ней имеется такая интересная фраза: 

> *Real asynchronous file operations with asyncio support*.  

А что там скрывается под Real — интересный вопрос.

 [Там](https://github.com/mosquito/aiofile/blob/master/aiofile/aio.py)  *не настоящая* асинхронная работа с файлами. Там используется  [run_in_executor](https://docs.python.org/3/library/asyncio-eventloop.html) . Еще  [передается](https://github.com/mosquito/aiofile/blob/master/aiofile/aio.py#L129)   None как параметр. Из этого следует, что кол-во потоков указано в ThreadPoolExecutor. В  [доке](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.ThreadPoolExecutor)  инфа по тому сколько потоков в нем. Получается обманули, не такая настоящая асинхронность, а обычный запуск на нескольких потоках 😓

Я как-то искал как работать с файлами *really-асинхронно*, но не нашел нормального *prod-grade* способа. Поэтому это, видимо, лучший способ, который *realизуем*. 

PS. Если кто подскажет как честно работать асинхронно с файлами — будет круто. Вроде есть  [Linux AIO](https://github.com/littledan/linux-aio) , но я не нашёл чтобы его пользовали прям где-то.
