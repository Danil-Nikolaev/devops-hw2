# Роль `postgres`

Роль устанавливает PostgreSQL из стандартного репозитория Ubuntu, создаёт отдельный кластер данных и systemd-службу, пользовательские базы и роли, а также устанавливает postgres-exporter.

Готовые роли PostgreSQL из Ansible Galaxy не используются. Коллекция `community.postgresql` нужна только для отдельных модулей управления объектами СУБД.

## Переменные роли

| Переменная | Значение по умолчанию | Назначение |
|---|---|---|
| `postgres__version` | `16` | Версия PostgreSQL из репозитория Ubuntu |
| `postgres__data_dir` | `/data/postgres/data` | Каталог кластера данных |
| `postgres__opts` | словарь | Параметры `postgresql.conf` |
| `postgres__hba_rules` | список словарей | Правила `pg_hba.conf` |
| `postgres__users` | `[]` | Пользователи PostgreSQL |
| `postgres__databases` | `[]` | Пользовательские базы данных |
| `postgres__exporter_version` | `0.20.1` | Версия postgres-exporter |
| `postgres__exporter_port` | `9187` | Порт метрик exporter |
| `postgres__exporter_db_user` | `postgres_exporter` | Пользователь БД для exporter |
| `postgres__exporter_db_password` | пустая строка | Пароль пользователя exporter; передавать через vault |

Элементы `postgres__users` поддерживают поля `name`, `password`, `role_attr_flags`, `state`. Элементы `postgres__databases` поддерживают `name`, `owner`, `encoding`, `state`.

## Пример плейбука

```yaml
---
- name: Install PostgreSQL on vm1
  hosts: vm1
  roles:
    - postgres
```

Пароли следует хранить в зашифрованном файле `group_vars` с помощью `ansible-vault`.

