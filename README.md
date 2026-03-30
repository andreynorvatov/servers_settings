# Servers Settings

Ansible-проект для управления серверами Ubuntu.

## Структура проекта

```
.
├── inventory/           # Инвентари для разных окружений
│   ├── production.ini   # Продакшен серверы
│   ├── staging.ini      # Тестовые серверы
│   └── group_vars/      # Переменные для групп
├── playbooks/           # Плейбуки Ansible
├── roles/               # Роли Ansible
├── group_vars/          # Глобальные переменные
└── host_vars/           # Переменные для отдельных хостов
```

## Быстрый старт

1. Скопируйте `.env.example` в `.env` и заполните переменными:
   ```bash
   cp .env.example .env
   ```

2. Настройте инвентарь в `inventory/production.ini` или `inventory/staging.ini`

3. Запустите плейбук:
   ```bash
   ansible-playbook -i inventory/production.ini playbooks/update_ubuntu.yml
   ```

## Требования

- Ansible >= 2.9
- Python >= 3.8

## Установка зависимостей

```bash
ansible-galaxy install -r requirements.yml
```

## Использование

### Обновление всех серверов

```bash
ansible-playbook -i inventory/production.ini playbooks/site.yml
```

### Обновление только Ubuntu серверов

```bash
ansible-playbook -i inventory/production.ini playbooks/update_ubuntu.yml
```

### Проверка подключения

```bash
ansible all -i inventory/production.ini -m ping
```

## Безопасность

- Не коммитьте файл `.env` с реальными паролями
- Используйте Ansible Vault для чувствительных данных
- Настройте SSH-ключи для аутентификации

## Лицензия

MIT
