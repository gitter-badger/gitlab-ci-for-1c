# gitlab-ci-for-1c

Готовый упрощенный шаблон для начального разворачивания обновления конфигурации и дополнительных отчетов и обработок для 1С:Предприятие 8 на Gitlab CI.

## Принципы работы (разработка)

**Пуш в master запрещен**

---

### Работа с конфигурацией

В develop или в feature-ветках работа идет "как обычно" - редактируем код, выгружаем конфигурацию в файлы

При коммите и отправке на gitlab в develop или при merge-request из фиче-ветки в девелоп запускается скрипт test_CanCompile.os, проверяющий корректность исходников конфигурации. На эту же задачу CI можно навесить различные TDD и BDD проверки.

> При желании работа ведется через хранилище, настраивается связка с gitsync.os

### Работа с обработками

Для сборки обработок в бинарники предназначен скрипт build_processings.cmd. Так же можно настроить post-recieve хук в git, чтобы обработки собирались автоматически при получении исходинков через git pull.  
Сборка обработок осуществляется иерархически от каталога src/processings.

Разбор обработок происходит скриптом decompose_processings.cmd  
Для этого обработки должны быть помещены в индекс git.
> Важно!  
Так как сам каталог processings целиком находится под gitignore, то помещение обработок в индекс выполняется из консоли с флагом -f, например: `git add -f processings/МояОбработка.epf`

В итоге под git оказываются только исходники конфигурации и исходники обработок.

## Деплой

При мерже в master-ветку запускается скрипт ci_deploy.os.

Скрипт на основании лога git получает список измененных файлов, обновляет конфигурацию БД (при необходимости), собирает измененные отчеты и обработки и помещает их в справочник "Дополнительные отчеты и обработки". Поиск обработок осуществляется по имени файла.

## Список проблем

* Инкрементальная выгрузка - в какой-то момент работала, потом пришлось отключить и загружать целиком. В скрипте заложена возможность получения конкретного списка измененных файлов и формирования корректной строки запуска конфигуратора в пакетном режиме.
* Конфигурация обновляется динамически. До сих пор не оттестировал работу обновления, требующего выгона пользователей. Вполне вероятно, что в конфигураторе просто повиснет модальное окно, так как каких-либо дополнительных флагов подавления сообщений скрипт на данный момент не пробрасывает.
* Загрузка обработок в справочник "Дополнительные отчеты и обработки" реализована на базе БСП 2.1.3.53. При использовании БСП 2.2 и 2.3 потребуется доработка напильником.
