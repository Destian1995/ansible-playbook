## Playbook

Playbook производит установку и настройку следующих приложений на указанных серверах.

- ### Clickhouse
  - установка `clickhouse`
  - настройка удаленных подключений к приложению
  - создание базы данных и таблицы в ней
- ### Vector
  - установка `vector`
  - изменение конфига приложения для отправки логов на сервер `clickhouse-01`


## Variables

Через group_vars можно задать следующие параметры:
- `clickhouse_version`, `vector_version` - версии устанавливаемых приложений;
- `clickhouse_database_name` - имя базы данных для хранения логов;
- `clickhouse_create_table` - структуру таблицы для хранения логов;
- `vector_config` - содержимое конфигурационного файла для приложения `vector`;

## Tags

- `clickhouse` производит полную конфигурацию сервера `clickhouse-01`;
- `clickhouse_db` производит конфигурацию базы данных и таблицы;
- `vector` производит полную конфигурацию сервера `vector-01`;
- `vector_config` производит изменение в конфиге приложения `vector`;
- `drop_clickhouse_database_logs` удаляет базу данных (по умолчанию не выполняется);

---

- Для изменения конфигурации `vector` достаточно использовать тэг `vector_config`

    ```console
    ansible-playbook -i inventory/prod.yml site.yml --tags vector_config
    ```

- для создания новой базы данных и записи логов в нее необходимо использовать тэги `clickhouse_db` и `vector_config`

    ```console
    ansible-playbook -i inventory/prod.yml site.yml --tags clickhouse_db --tags vector_config
    ```

- если требуется пересоздать существующую базу данных, то можно использовать тэги `drop_clickhouse_database_logs` и `clickhouse_db`
- 
    ```console
    ansible-playbook -i inventory/prod.yml site.yml --tags drop_clickhouse_database_logs --tags clickhouse_db
    ```
Для полной установки инфраструктуры указывать тэги не требуется.