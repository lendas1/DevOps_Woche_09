# Nextcloud with MariaDB

## Voraussetzungen

- Docker
- Docker Compose

## Verwendete Images

- Nextcloud: `nextcloud:25.0.3`
- MariaDB: `mariadb:10.9`

## Installation

1. Ziehe die Docker Images von Docker Hub:

    ```sh
    docker pull nextcloud:25.0.3
    docker pull mariadb:10.9
    ```

2. Erstelle die folgenden Dateien in einem Projektverzeichnis:

    - `docker-compose.yml`

3. Inhalt der `docker-compose.yml`:

    ```yaml
    version: '3.8'

    services:
      db:
        image: mariadb:10.9
        container_name: mariadb
        restart: always
        environment:
          MYSQL_ROOT_PASSWORD: example
          MYSQL_DATABASE: nextcloud
          MYSQL_USER: nextcloud
          MYSQL_PASSWORD: example
        volumes:
          - db_data:/var/lib/mysql

      app:
        image: nextcloud:25.0.3
        container_name: nextcloud
        ports:
          - "8080:80"
        restart: always
        environment:
          MYSQL_HOST: db
          MYSQL_DATABASE: nextcloud
          MYSQL_USER: nextcloud
          MYSQL_PASSWORD: example
        depends_on:
          - db
        volumes:
          - nextcloud_data:/var/www/html

    volumes:
      db_data: {}
      nextcloud_data: {}
    ```

4. Starte die Container:

    ```sh
    docker-compose up -d
    ```

5. Überprüfe die laufenden Container:

    ```sh
    docker ps
    ```

6. Besuche `http://localhost:8080` in deinem Browser, um auf die Nextcloud-Installationsseite zuzugreifen.

7. Schließe die Einrichtung von Nextcloud im Browser ab.

## Hinweise

- Die MySQL-Passwörter und andere Umgebungsvariablen können nach Bedarf geändert werden.
- Du kannst die `docker-compose.yml` Datei anpassen, um die Ressourcenbeschränkungen hinzuzufügen, falls erforderlich.
