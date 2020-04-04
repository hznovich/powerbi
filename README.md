---
permalink: /index.html
---

# Metrika Logs API > Power Bi & Excel

Это инструкция к файлу https://github.com/meta110/powerbi/blob/master/raw_metrika_methods

Функция напрямую работает с Logs API Яндекс Метрики https://yandex.ru/dev/metrika/doc/api2/logs/intro-docpage/. Для одного счетчика Яндекс.Метрики суммарный размер данных, передаваемых в запросах (включая не удаленные из хранилища логи), составляет 10 ГБ.

Видео о том, как работать с коннектором:
 [![Видео о том, как работать с коннектором](https://lh5.googleusercontent.com/PBJAGWnccaArt3NMEtQA8BvlwE0P07WZ7xs94HVlU0b-OC41CsgDA_dZKXtzTS1oP2WtFSs3sY4RRudzWP-XV_O3cN8GlnAJ9vGhh8cLRCXaYmdo2weiZQoZe9RCRAsaD4z_T0vE)](https://youtu.be/BZhgrGVJ1SY)


Параметр|Описание
--------|--------
Метод | Указывает тип запроса к Метрике. См. далеее
Идентификатор запроса ( ID запроса) | Идентификатор запроса к Logs API. Обязателен для методов вам известен идентификатор запроса
Дата начала интервала | Начальная дата периода отчета
Дата окончания интервала | Последняя дата периода отчета. Не может быть сегодняшним числом
Номер счетчика Яндекс.Метрики | Номер счетчика, по которому хотите получить статистику
Ключ авторизации Яндекс.Метрики | API-ключ авторизации. Залогиньтесь в нужном аккаунте Метрики и получите ключ по ссылке https://oauth.yandex.ru/authorize?response_type=token&client_id=764f4af41256427ba87965a7ed31ea3d
Тип отчета |visits - по визитам https://yandex.ru/dev/metrika/doc/api2/logs/fields/visits-docpage/ , hits - по просмотрам https://yandex.ru/dev/metrika/doc/api2/logs/fields/hits-docpage/
Список полей | Список полей согласно выбранному типу отчета в виде списка list или в виде строки через запятую

## Описание методов

Метод | ID запроса | Описание
------|---------|-----
evaluate|-|Проверить возможность создания запроса логов для указанного аккаунта с выбранными типом, периодом и списком полей отчета. Реализует метод https://yandex.ru/dev/metrika/doc/api2/logs/queries/evaluate-docpage/
create|-|Создать запрос логов для указанного аккаунта с выбранными типом, периодом и списком полей отчета. Расширенная реализация метода https://yandex.ru/dev/metrika/doc/api2/logs/queries/createlogrequest-docpage/ : прежде чем создать запрос, проверяет, что в аккаунте нет аналогичных отчетов по периоду дат, типу отчета и полям. Если находит, то возвращает его идентификатор
list|-|Отобразить список запросов и отчетов в аккаунте. Реализует метод https://yandex.ru/dev/metrika/doc/api2/logs/queries/getlogrequests-docpage/
info|обязателен|Запросить статус отчета с указанным идентификатором (список статусов представлен ниже). Реализует метод https://yandex.ru/dev/metrika/doc/api2/logs/queries/getlogrequest-docpage/
download|обязателен|Загрузить готовый отчет (со статусом processed). Расширенная реализация метода https://yandex.ru/dev/metrika/doc/api2/logs/queries/download-docpage/ : полностью загружает все части. Если отчет не готов, то ответ будет аналогичен методу info
clean|обязателен|Удалить готовый отчет с указанным идентификатором. Реализует метод https://yandex.ru/dev/metrika/doc/api2/logs/queries/clean-docpage/
cancel|обязателен|Отменить не подготовленный отчет с указанным идентификатором. Реализует метод https://yandex.ru/dev/metrika/doc/api2/logs/queries/cancel-docpage/
cleanall|-|Полностью очищает все запросы логов и готовые отчеты в аккаунте с помощью комбинации методов clean и cancel
auto|-|В автоматическом режиме проверяет возможность создания запроса, создает запрос, дожидается подготовки отчета и загружает готовый отчет. Если аналогичный отчет уже есть, то загружается он


Отчеты подготавливаются в течение некоторого времени от нескольких минут и более. Возможные статусы отчета:
* processed — обработан (подготовлен).
* canceled — отменён.
* processing_failed — ошибка при обработке.
* created — создан.
* cleaned_by_user — очищен пользователем.
* cleaned_automatically_as_too_old — очищен автоматически.
