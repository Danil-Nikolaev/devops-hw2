# Домашнее задание 2 — Ansible и PostgreSQL

Проект содержит собственную роль `postgres`, плейбук и inventory для установки PostgreSQL на `vm1`.

## Подготовка

```bash
ansible-galaxy collection install -r requirements.yml
ansible-inventory --list
ansible vm1 -m ping
```

Файл `.vault-pass` хранится только локально и исключён из Git. Зашифрованные переменные находятся в `inventories/main/group_vars/all/vault.yml`.

## Запуск

```bash
ansible-playbook postgres.play.yml
```

Повторный запуск должен завершиться с `changed=0`.

## Проверка

```bash
ansible vm1 -b -m command -a 'systemctl is-active postgresql-hw2 postgres-exporter'
ansible vm1 -b -m command -a 'systemctl is-enabled postgresql-hw2 postgres-exporter'
ansible vm1 -b --become-user postgres -m command -a 'psql -c "SELECT 1"'
ansible vm1 -b --become-user postgres -m command -a 'psql -c "\l"'
ansible vm1 -b --become-user postgres -m command -a 'psql -c "\du"'
ansible vm1 -m uri -a 'url=http://127.0.0.1:9187/metrics return_content=true'
```

