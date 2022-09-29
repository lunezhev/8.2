# Домашнее задание к занятию "08.02 Работа с Playbook"

## Подготовка к выполнению

1. (Необязательно) Изучите, что такое [clickhouse](https://www.youtube.com/watch?v=fjTNS2zkeBs) и [vector](https://www.youtube.com/watch?v=CgEhyffisLY)
2. Создайте свой собственный (или используйте старый) публичный репозиторий на github с произвольным именем.
3. Скачайте [playbook](./playbook/) из репозитория с домашним заданием и перенесите его в свой репозиторий.
4. Подготовьте хосты в соответствии с группами из предподготовленного playbook.

## Основная часть

1. Приготовьте свой собственный inventory файл `prod.yml`.
2. Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает [vector](https://vector.dev).
3. При создании tasks рекомендую использовать модули: `get_url`, `template`, `unarchive`, `file`.
4. Tasks должны: скачать нужной версии дистрибутив, выполнить распаковку в выбранную директорию, установить vector.
5. Запустите `ansible-lint site.yml` и исправьте ошибки, если они есть.
```
ad@k8s:~/8.2$ ansible-lint playbook/site.yml
WARNING  Overriding detected file kind 'yaml' with 'playbook' for given positional argument: playbook/site.yml
WARNING  Listing 3 violation(s) that are fatal
name: All tasks should be named. (name[missing])
playbook/site.yml:11 Task/Handler: block/always/rescue 

jinja: Jinja2 spacing could be improved: create_db.rc != 0 and create_db.rc !=82 -> create_db.rc != 0 and create_db.rc != 82 (jinja[spacing])
playbook/site.yml:36 Jinja2 template rewrite recommendation: `create_db.rc != 0 and create_db.rc != 82`.

yaml: no new line character at the end of file (yaml[new-line-at-end-of-file])
playbook/site.yml:79

You can skip specific rules or tags by adding them to your configuration file:
# .config/ansible-lint.yml
warn_list:  # or 'skip_list' to silence them completely
  - name[missing]  # Rule for checking task and play names.
  - yaml[new-line-at-end-of-file]  # Violations reported by yamllint.

Finished with 2 failure(s), 1 warning(s) on 1 files.
ad@k8s:~/8.2$ sed -i -e '$a\' playbook/site.yml
ad@k8s:~/8.2$ ansible-lint playbook/site.yml
WARNING  Overriding detected file kind 'yaml' with 'playbook' for given positional argument: playbook/site.yml
WARNING  Listing 2 violation(s) that are fatal
name: All tasks should be named. (name[missing])
playbook/site.yml:11 Task/Handler: block/always/rescue 

jinja: Jinja2 spacing could be improved: create_db.rc != 0 and create_db.rc !=82 -> create_db.rc != 0 and create_db.rc != 82 (jinja[spacing])
playbook/site.yml:36 Jinja2 template rewrite recommendation: `create_db.rc != 0 and create_db.rc != 82`.

You can skip specific rules or tags by adding them to your configuration file:
# .config/ansible-lint.yml
warn_list:  # or 'skip_list' to silence them completely
  - name[missing]  # Rule for checking task and play names.

Finished with 1 failure(s), 1 warning(s) on 1 files.

```
Исправил отсутствие имени в 11 строке:
```
  tasks:
    - name: Get clickhouse distrib
      block:
        - name: Get clickhouse distrib
```
```
ad@k8s:~/8.2$ ansible-lint playbook/site.yml
WARNING  Overriding detected file kind 'yaml' with 'playbook' for given positional argument: playbook/site.yml
WARNING  Listing 1 violation(s) that are fatal
jinja: Jinja2 spacing could be improved: create_db.rc != 0 and create_db.rc !=82 -> create_db.rc != 0 and create_db.rc != 82 (jinja[spacing])
playbook/site.yml:37 Jinja2 template rewrite recommendation: `create_db.rc != 0 and create_db.rc != 82`.


Finished with 0 failure(s), 1 warning(s) on 1 files.
```


6. Попробуйте запустить playbook на этом окружении с флагом `--check`.
7. Запустите playbook на `prod.yml` окружении с флагом `--diff`. Убедитесь, что изменения на системе произведены.
8. Повторно запустите playbook с флагом `--diff` и убедитесь, что playbook идемпотентен.
9. Подготовьте README.md файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги.
10. Готовый playbook выложите в свой репозиторий, поставьте тег `08-ansible-02-playbook` на фиксирующий коммит, в ответ предоставьте ссылку на него.

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
