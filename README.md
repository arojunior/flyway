# Docker FlyWay

[FlyWay command-line tool](https://flywaydb.org/getstarted/firststeps/commandline) with wait-for-it command docker image.

wait-for-it command allows us to wait for database to set up before using FlyWay to migrate.

### Usage

conf/flyway.conf file:
`flyway.url=jdbc:mysql://localhost:3306/some_db`
`flyway.user=some_user`
`flyway.password=another_secure_password`

#### Command
Command using conf/flyway.conf file:
`docker run -v $(pwd)/flyway/conf:/flyway/conf -v $(pwd)/sql:/flyway/sql arojunior/flyway:5.2.4 flyway migrate `

Command with arguments:
`docker run -v $(pwd)/sql:/flyway/sql arojunior/flyway:5.2.4 flyway -url=jdbc:mysql://localhost:3306/mydb -user=user -password=password migrate`

#### Docker compose

Here flyway will wait for mysql to set up before executing migration.

```yml
version: '2'
services:
  mysql:
    image: mysql:5.7
    ports:
      - '3306:3306'
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: mydb
      MYSQL_USER: user
      MYSQL_PASSWORD: password
  flyway:
    image: arojunior/flyway:5.2.4
    command: "wait-for-it --timeout=30 --strict mysql:3306 -- flyway migrate"
    volumes:
      - './flyway/conf:/flyway/conf'
      - './sql:/flyway/sql'
```


### Credits
- [Boxfuse](https://boxfuse.com/) (FlyWay)
- [Giles Hall (vishnubob)](https://github.com/vishnubob/wait-for-it) (wait-for-it command)
- [jimmyoak](https://github.com/jimmyoak/flyway) (forked)
