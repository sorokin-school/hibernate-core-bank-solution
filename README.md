# Hibernate Core Bank Solution

Консольное учебное банковское приложение на Java + Spring Core + Hibernate Core. Данные хранятся в PostgreSQL.

## Что умеет
- создавать пользователей;
- показывать всех пользователей и их счета;
- создавать дополнительные счета;
- пополнять и снимать деньги;
- переводить между счетами (с комиссией для разных пользователей);
- закрывать счет с переносом остатка;
- завершать работу по команде `EXIT`.

## Технологии
- Java 21
- Spring Core (`spring-context`)
- Конфигурация через `@Configuration`, `@PropertySource`, `@Component`
- Hibernate ORM **7.4.15.Final** (`SessionFactory`, `Session`, ручное управление транзакциями)
- PostgreSQL **16**

## Архитектура
- `User`, `Account` — Hibernate-сущности.
- `UserService`, `AccountService` — бизнес-логика и работа с БД.
- `OperationCommand` + `ConsoleOperationType` — обработка команд (Command pattern).
- `OperationsConsoleListener` — главный цикл приложения.
- `ConsoleInput` — единая точка чтения/валидации консольного ввода.

## Команды
- `USER_CREATE`
- `SHOW_ALL_USERS`
- `ACCOUNT_CREATE`
- `ACCOUNT_DEPOSIT`
- `ACCOUNT_WITHDRAW`
- `ACCOUNT_TRANSFER`
- `ACCOUNT_CLOSE`
- `EXIT`

## Настройки
Файл: `src/main/resources/application.properties`

```properties
account.default-amount=500
account.transfer-commission=0.02
```

## Запуск

1. Проверьте JDK21: `java -version` и `mvn -version`. Maven должен использовать тот же JDK.
2. Поднимите учебную PostgreSQL16 с базой `bank`. Например, через Docker:
```bash
docker run --name minibank-postgres \
  -e POSTGRES_DB=bank \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=root \
  -p 127.0.0.1:5432:5432 \
  -d postgres:16
```
Параметры подключения для этого примера: `db.url=jdbc:postgresql://localhost:5432/bank`, `db.username=postgres`, `db.password=root`. Если БД уже запущена, используйте её адрес и существующие данные. Пароль `root` — только для локального учебного примера.
3. В `src/main/resources/application.properties` проверьте `db.*`, `hibernate.*` и `account.*`. В частности, нужны `hibernate.current_session_context_class=thread` и `hibernate.hbm2ddl.auto=update`.
4. Соберите проект:
```bash
mvn clean package
```
5. Запустите:
```bash
mvn exec:java -Dexec.mainClass="sorokin.java.course.Main"
```

Если `exec-maven-plugin` не настроен, можно запускать из IDE через класс `Main`.
