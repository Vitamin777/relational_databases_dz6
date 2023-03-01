# Домашнее задание к занятию 
# "`Резервное копирование`" - `Тимохин Виталий`

### Задание 1. Резервное копирование

### Кейс.
Финансовая компания решила увеличить надёжность работы баз данных и их резервного копирования.

Необходимо описать, какие варианты резервного копирования подходят в случаях:

#### Задание 1.1

Необходимо восстанавливать данные в полном объёме за предыдущий день.

`Решение:`

`Необходимо выполнять общие рекомендации со слайда 37 лекции:`  
`Традиционно рекомендуют держать 10 бэкапов: по одному на каждый день недели, а также бэкапы двухнедельной, месячной и квартальной давности — это позволит достаточно глубоко откатиться в случае порчи каких-либо данных. Храниться бэкапы должны точно не на том же диске, что и живая база, и не на том же сервере.`

`Так же необходимо осуществлять бэкапы:`

1. `Полный бэкап (Full backup) раз в неделю (в ночь с воскресенья на понедельник).`

2. `Дифференциальный бэкап (differential backup) концом каждых суток. Если нагрузка на систему не позволяет осуществлять дифференциальный бэкап, то осуществляем инкрементный бэкап (Incremental backup) концом каждых суток.`

#### Задание 1.2 

Необходимо восстанавливать данные за час до предполагаемой поломки.

`Решение:`

`Выполняем общие рекомендации, описанные в в решении задачи 1.1.`

`Так же необходимо осуществлять бэкапы:`

1. `Полный бэкап (Full backup) раз в неделю (в ночь с воскресенья на понедельник).`

2. `Дифференциальный бэкап (differential backup) концом каждых суток.`

3. `Инкрементный бэкап (Incremental backup) концом каждого часа.`

---

### Задание 2. PostgreSQL.

#### Задание 2.1

С помощью официальной документации приведите пример команды резервирования данных и восстановления БД (pgdump/pgrestore).

`Решение:`

`Выгрузим базу данных mydb в файл специального формата:`

```
$ pg_dump -Fc mydb > db.dump
```

`Мы можем удалить базу данных и восстановить её из копии:`

```
$ dropdb mydb
$ pg_restore -C -d postgres db.dump
```

`В аргументе -d можно указать любую базу данных, существующую в кластере; pg_restore использует её, только чтобы выполнить команду CREATE DATABASE для базы mydb. С параметром -C данные всегда восстанавливаются в базу, имя которой записано в файле архива.`

---

### Задание 3. MySQL.

#### Задание 3.1
С помощью официальной документации приведите пример команды инкрементного резервного копирования базы данных MySQL.

`Решение:`

`В MySQL для резервного копирования применяется утилита  mysqlbackup.`

`Ниже приведен пример команды инкрементного резервного копирования базы данных MySQL.`

```
mysqlbackup \
	--incremental \
	--incremental-base=history:last_backup \ 
	--backup-dir=/home/dbadmin/temp_dir \
	--backup-image=incremental_image1.bi
``` 

`где опции:`

| 1 | 2 | 3 | 4 |
| ---- | ---- | ---- | ---- |
| Один | Два | Три | Четыре |
| One | Two | Free | For |
| 一 | 二 | 三 | 四 |


|--incremental|- указывает на то, что будет осуществляться инкрементное резервное копирование.|
|--incremental-base=history:last_backup|- указывает на то, что инкрементное резервное копирование будет осуществляться относительно последнего успешного резервного копирования.|
|--backup-dir=/home/dbadmin/temp_dir|- указывает на директорию, в которую будет помещен backup файл по завершению резервного копирования .|
|--backup-image=incremental_image1.bi|- указывает имя backup файла.|
