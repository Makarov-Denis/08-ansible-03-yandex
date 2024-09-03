
### Макаров Денис 

### Руководство по использованию Ansible Playbook для установки ClickHouse и Vector и Lighthouse

Описание

Этот Ansible Playbook предназначен для автоматической установки и настройки баз данных ClickHouse, системы логирования Vector, а также инструмента аудита производительности веб-страниц Lighthouse на удаленные серверa. Сценарий выполняет загрузку необходимых пакетов, их установку, а также создание и настройку конфигурационных файлов.

Содержимое

    site.yml: Основной Playbook, который включает в себя установку и настройку ClickHouse и Vector.
    group_vars/lighthouse/clickhouse_vars.yml: Файл переменных, для настройки ClickHouse
    group_vars/vector/vector_vars.yml: Файл переменных, для настройки vector.
    vector.yml.j2: Jinja2-шаблон для конфигурационного файла Vector.
    inventory/prod.yml: Инвентори-файл, содержащий описание хостов для развёртывания.
    group_vars/lightHouse/lighthouse_vars.yml: Файл переменных для настройки Lighthouse.
    lighthouse_nginx.conf.j2: Jinja2-шаблон для конфигурационного файла Lighthouse.

Требования

    Ansible 2.9 или выше
    Доступ к серверам с установленной операционной системой на базе RedHat/CentOS
    Настроенный SSH-доступ к серверам

Шаги по установке
Подготовка серверов:

    Убедитесь, что на серверах установлены все необходимые зависимости, такие как Python.
    Настройте SSH-доступ к серверам, указав приватный ключ в inventory/prod.yml.
Настройка переменных:

Откройте файл vars.yml и измените значения переменных, если необходимо:
    clickhouse_version: Версия ClickHouse.
    clickhouse_packages: Список пакетов ClickHouse для установки.
    vector_version: Версия Vector.
    vector_config_dir: Путь к каталогу конфигурации Vector.
    vector_config: Конфигурация источников и потребителей данных в Vector.
    nodejs_version: Версия Node.js для установки в процессе настройки Lighthouse.

Запуск Playbook:

Выполните следующую команду для запуска Playbook на удаленных серверах:

ansible-playbook -i inventory/prod.yml site.yml

    Проверка установки:

    После завершения выполнения Playbook убедитесь, что службы ClickHouse и Vector запущены корректно:
        Для ClickHouse: подключитесь к серверу и выполните команду clickhouse-client, чтобы проверить доступность базы данных logs.
        Для Vector: убедитесь, что служба запущена с помощью команды systemctl status vector.
        Для Lighthouse: убедитесь, что инструмент установлен и доступен для использования командой lighthouse.

Детали выполнения

    ClickHouse:
        Playbook загружает и устанавливает необходимые RPM-пакеты ClickHouse.
        Служба clickhouse-server автоматически перезапускается после установки.
        Создаётся база данных logs.

    Vector:
        Playbook загружает и устанавливает RPM-пакет Vector.
        Применяется конфигурация, описанная в файле vector.yml.j2.
        Служба vector автоматически перезапускается после применения новой конфигурации.

    Lighthouse:
        Playbook загружает и устанавливает необходимые пакеты для работы с Node.js, включая gcc-c++, make и curl.
        Устанавливается и настраивается Node Version Manager (nvm) для управления версиями Node.js.
        Устанавливается указанная версия Node.js и инструмент Lighthouse через npm.
        Создаётся каталог конфигурации для Lighthouse в /etc/lighthouse с правами доступа 0755.
        Копируется конфигурационный файл Lighthouse из шаблона lighthouse-config.json.j2 в /etc/lighthouse/config.json с правами доступа 0644.
        В случае необходимости Playbook перезапускает службу Lighthouse после применения конфигурации.

Примечания

    Если установка пакета ClickHouse не удалась на этапе загрузки, Playbook попытается загрузить альтернативный пакет в режиме rescue.
    Конфигурация Vector и Lighthouse создается на основе шаблона vector.yml.j2, где можно гибко изменять настройки источников и приемников данных.
    В случае необходимости, вы можете изменить параметры служб ClickHouse, Vector и Lighthouse, редактируя соответствующие файлы конфигурации и перезапустив Playbook.

Поддержка
Если у вас возникли вопросы или проблемы с этим Playbook, пожалуйста, создайте issue в соответствующем репозитории или обратитесь к администратору вашей системы.

Решение: 

Выполнение задания представлено на скриншотах ниже, а также в загруженных файлах.

![ansible-lint](https://github.com/user-attachments/assets/371a305b-1e6b-4ef2-a84f-5ad80726c5cf)

![ansible-check](https://github.com/user-attachments/assets/2ff0bcc1-acc4-4096-b038-791f72e6a49f)

![playbook](https://github.com/user-attachments/assets/1a2d5c37-7570-4dfa-8f0f-254d4cd1f30e)

![2](https://github.com/user-attachments/assets/93d22516-4d1c-49d3-9fda-e04d8997eb71)




