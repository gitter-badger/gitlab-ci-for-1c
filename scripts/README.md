## scripts

Каталог предназначен для хранения различных скриптов, необходимых для работы с репозиторием.

* build_processings.cmd - позволяет по щелчку мыши собрать внешние обработки из исходников в бинарники и положить их в каталог processings
* decompose_processings.cmd - позволяет по щелчку мыши разложить добавленные в индекс git внешние обработки в исходники внутрь папки src/processings


* ci_before_script.os - скрипт на OneScript, выполняемый перед началом сборки на Gitlab CI
* test_CanCompile.os - скрипт на OneScript, проверяющий, что текущие исходники конфигурации могут собраться в валидную конфигурацию
* ci_deploy.os - скрипт на OneScript, выполняющий обновление конфигурации на боевой базе и обновление внешних отчетов и обработок в справочнике "Дополнительные отчеты и обработки"
