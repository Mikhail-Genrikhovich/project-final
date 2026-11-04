## [REST API](http://localhost:8080/doc)

## Концепция:

- Spring Modulith
  - [Spring Modulith: достигли ли мы зрелости модульности](https://habr.com/ru/post/701984/)
  - [Introducing Spring Modulith](https://spring.io/blog/2022/10/21/introducing-spring-modulith)
  - [Spring Modulith - Reference documentation](https://docs.spring.io/spring-modulith/docs/current-SNAPSHOT/reference/html/)

```
  url: jdbc:postgresql://localhost:5432/jira
  username: jira
  password: JiraRush
```

- Есть 2 общие таблицы, на которых не fk
  - _Reference_ - справочник. Связь делаем по _code_ (по id нельзя, тк id привязано к окружению-конкретной базе)
  - _UserBelong_ - привязка юзеров с типом (owner, lead, ...) к объекту (таска, проект, спринт, ...). FK вручную будем
    проверять

## Аналоги

- https://java-source.net/open-source/issue-trackers

## Тестирование

- https://habr.com/ru/articles/259055/

## Список выполненных задач

1. Onboarding - разобрался со структурой проекта.
2. Удалил соцсети VK и Yandex (настройки, кнопки на страницах входа и регистрации, классы-обработчики).
3. Вынес секреты в переменные окружения (список ниже).
4. Тесты теперь идут на H2 в памяти, а не на PostgreSQL. Два бина `DataSource` выбираются по профилю (`DataSourceConfig`): для `test` - H2, для остальных - PostgreSQL.
6. Переписал `FileUtil#upload` на `java.nio` (`Path`, `Files`).

Остальные задания не делал.

## Переменные окружения

Нужны для запуска приложения с профилем `prod`:

`DB_USER`, `DB_PASSWORD` (по умолчанию `jira` / `JiraRush`), `GITHUB_CLIENT_ID`, `GITHUB_CLIENT_SECRET`, `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, `GITLAB_CLIENT_ID`, `GITLAB_CLIENT_SECRET`, `MAIL_USERNAME`, `MAIL_PASSWORD`

Для сборки и тестов они не нужны.

## Запуск

1. `docker compose up -d` - поднимает PostgreSQL.
2. `mvn clean install` - сборка и тесты (профиль Maven `prod` не включать, он пропускает тесты).
3. Запустить `JiraRushApplication` с профилем `prod` и переменными окружения.