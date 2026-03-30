# Команды Ansible

Полезные команды для работы с Ansible-проектом.

---

## 🔍 Проверка подключения

```bash
# Проверка доступности всех серверов
ansible all -i inventory/production.ini -m ping

# Проверка конкретного хоста
ansible server1 -i inventory/production.ini -m ping

# Проверка с парольной аутентификацией
ansible all -i inventory/production.ini -m ping --ask-pass

# Проверка с sudo-паролем
ansible all -i inventory/production.ini -m ping --ask-become-pass
```

---

## ▶️ Запуск плейбуков

### Основные команды

```bash
# Запуск плейбука на всех серверах
ansible-playbook -i inventory/production.ini playbooks/site.yml

# Запуск на конкретном хосте
ansible-playbook -i inventory/production.ini playbooks/site.yml --limit server1

# Запуск на нескольких хостах
ansible-playbook -i inventory/production.ini playbooks/site.yml --limit server1,server2

# Запуск по группе серверов
ansible-playbook -i inventory/production.ini playbooks/site.yml --limit ubuntu_servers
```

### Режимы проверки

```bash
# Dry-run (проверка без реальных изменений)
ansible-playbook -i inventory/production.ini playbooks/site.yml --check

# Показать различия (diff)
ansible-playbook -i inventory/production.ini playbooks/site.yml --check --diff

# Подробный вывод (-v, -vv, -vvv)
ansible-playbook -i inventory/production.ini playbooks/site.yml -v
ansible-playbook -i inventory/production.ini playbooks/site.yml -vvv
```

### Теги и задачи

```bash
# Запуск только задач с определённым тегом
ansible-playbook -i inventory/production.ini playbooks/site.yml --tags "security"

# Пропуск задач с определённым тегом
ansible-playbook -i inventory/production.ini playbooks/site.yml --skip-tags "update"

# Запуск с определённой задачи (по имени или ID)
ansible-playbook -i inventory/production.ini playbooks/site.yml --start-at-task="Обновление кэша apt"
```

---

## 🔐 Аутентификация

```bash
# SSH-пароль
ansible-playbook -i inventory/production.ini playbooks/site.yml --ask-pass

# Sudo-пароль
ansible-playbook -i inventory/production.ini playbooks/site.yml --ask-become-pass

# Оба пароля
ansible-playbook -i inventory/production.ini playbooks/site.yml --ask-pass --ask-become-pass

# Использование конкретного SSH-ключа
ansible-playbook -i inventory/production.ini playbooks/site.yml --private-key ~/.ssh/mykey
```

---

## 📦 Управление ролями

```bash
# Установка ролей из requirements.yml
ansible-galaxy install -r requirements.yml

# Установка конкретной роли
ansible-galaxy install geerlingguy.security

# Установка в кастомную директорию
ansible-galaxy install -r requirements.yml -p roles/

# Список установленных ролей
ansible-galaxy list

# Удаление роли
ansible-galaxy remove geerlingguy.security
```

---

## 🧹 Очистка и обслуживание

```bash
# Очистка кэша фактов
ansible all -i inventory/production.ini -m ansible.builtin.meta -a "clear_facts"

# Сбор фактов о серверах
ansible all -i inventory/production.ini -m setup

# Фильтрация фактов
ansible all -i inventory/production.ini -m setup -a "filter=ansible_distribution*"
```

---

## 📊 Инвентарь и хосты

```bash
# Показать список всех хостов
ansible-inventory -i inventory/production.ini --list

# Показать график зависимостей групп
ansible-inventory -i inventory/production.ini --graph

# Проверка переменных для хоста
ansible server1 -i inventory/production.ini -m debug -a "var=hostvars[inventory_hostname]"
```

---

## 🛠️ Отладка

```bash
# Показать все переменные для хоста
ansible server1 -i inventory/production.ini -m debug -a "var=hostvars[inventory_hostname]"

# Показать конкретную переменную
ansible server1 -i inventory/production.ini -m debug -a "var=ansible_user"

# Проверка синтаксиса плейбука
ansible-playbook -i inventory/production.ini playbooks/site.yml --syntax-check

# Показать список задач в плейбуке
ansible-playbook -i inventory/production.ini playbooks/site.yml --list-tasks

# Показать список хостов, которые будут затронуты
ansible-playbook -i inventory/production.ini playbooks/site.yml --list-hosts
```

---

## 📁 Работа с разными окружениями

```bash
# Production окружение
ansible-playbook -i inventory/production.ini playbooks/site.yml

# Staging окружение
ansible-playbook -i inventory/staging.ini playbooks/site.yml

# Локальный тест (localhost)
ansible-playbook -i inventory/staging.ini playbooks/site.yml --connection=local
```

---

## ⚡ Быстрые команды для вашего проекта

```bash
# === ПРОВЕРКА ===
ansible all -i inventory/production.ini -m ping

# === ОБНОВЛЕНИЕ ВСЕХ СЕРВЕРОВ ===
ansible-playbook -i inventory/production.ini playbooks/site.yml

# === ТОЛЬКО ОБНОВЛЕНИЕ UBUNTU ===
ansible-playbook -i inventory/production.ini playbooks/update_ubuntu.yml

# === ПРОВЕРКА БЕЗ ИЗМЕНЕНИЙ ===
ansible-playbook -i inventory/production.ini playbooks/site.yml --check

# === ЗАПУСК НА ОДНОМ СЕРВЕРЕ ===
ansible-playbook -i inventory/production.ini playbooks/site.yml --limit server1

# === ПОДРОБНЫЙ ВЫВОД ===
ansible-playbook -i inventory/production.ini playbooks/site.yml -vvv
```

---

## 📝 Шпаргалка по параметрам

| Параметр | Короткая форма | Описание |
|----------|----------------|----------|
| `--inventory` | `-i` | Путь к инвентарю |
| `--limit` | `-l` | Ограничить хосты |
| `--check` | `-C` | Dry-run режим |
| `--diff` | | Показать различия |
| `--verbose` | `-v` | Подробный вывод |
| `--ask-pass` | `-k` | Запрос SSH-пароля |
| `--ask-become-pass` | `-K` | Запрос sudo-пароля |
| `--tags` | `-t` | Запустить задачи по тегу |
| `--skip-tags` | | Пропустить задачи по тегу |
| `--syntax-check` | | Проверка синтаксиса |
| `--list-tasks` | | Список задач |
| `--list-hosts` | | Список хостов |
