Разбил playbook на отдельные файлы (заню, что include deprecated, но пока пришло в голову только это):
tasks/add_user.yml -- добавляет пользователя ubuntu, назначает права и прописывает публичный ключ
tasks/install_webserver.yml -- устанавливает необходимые пакеты, запускает сервисы веб-сервера
tasks/laravel.yml -- запускает приложение
tasks/mysql.yml -- создает БД и пользователя с правами
tasks/ufw.yml -- настраивает ufw (открывает 22, 80 и 443 порты)

Как видно, куча файлов, которые тупо копируются (конфиги nginx и .env laravel) на целевой сервер, решение мягко говоря не оптимальное, можно было бы сделать шаблонами, но опыта и времени не хватило.
Сильно заставила понервничать работа с mysql (никак не давалась авторизация под рутом для создания БД), хотел остановиться уже на башсибле, прописав всё в модуле shell, но это совсем не наш метод -- в итоге нашел решение, индемподентное

Что касается теории:
**1. Как из данной архитектуры можно реализовать отказоустойчивый ресурс?**
Я бы для реализаци отказоустойчивого ресурса обратился к кубернетису -- он бы сам следил за нагрузкой на сервер и автоскейлил инстансы, если того требует нагрузка. Опять же также автоматом поднимал упавшие поды.
Или же поднял парочку виртуалок в разных зонах доступности, на них развернул приложение (можно и докеризировать его), балансировщиком распределял нагрузку на серверах

**2. Какие способы создания реплики от работающей Mysql базы вы знаете? В чём между ними различия? Какой из способов стоит использовать и почему? Не нужно перечислять типы репликации и писать о различиях. Нужно описать пошагово процесс создания реплики от работающей БД, кратко.**
С этим вопросом не знаком, не приходилось в своей практике реплицировать базы. Беглый гуглёж вопроса приводит меня к мысли, что для отказоустойчивой репликации лучше использовать метод Group Replication, но в принципе все зависит от задачи -- в каждом методе свои компромиссы
