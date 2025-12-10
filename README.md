###  SmartHouse – Application Smart Home (Spring Boot + Angular + Docker)
**Description du projet**

**SmartHouse est une application complète de gestion de maison intelligente (Smart Home).**

**Elle permet :**

- la gestion des devices (lampes, capteurs, objets connectés, etc.)

- la gestion des catégories

- l’ajout de photos

- l’activation / désactivation d’un appareil

- le tout avec un backend Spring Boot, un frontend Angular, et une base de données MySQL.

- Le projet est entièrement containerisé avec Docker et orchestré via docker-compose.

**Architecture du projet**

```text
smarthouse/
│
├── Smart_Home_back/        # Backend Spring Boot
│   ├── src/
│   ├── pom.xml
│   └── Dockerfile
│
├── smartHome-front/        # Frontend Angular
│   ├── src/
│   ├── package.json
│   └── Dockerfile
│
├── docker-compose.yml      # Orchestration Docker
└── README.md               # Documentation du projet
```



## Technologies utilisées


| Composant       | Technologie                                                |
| --------------- | ---------------------------------------------------------- |
| Backend         | Java 17, Spring Boot (REST API, JPA, Multipart, Hibernate) |
| Frontend        | Angular + TypeScript + HTML/CSS                            |
| Base de données | MySQL 8                                                    |
| Interface DB    | phpMyAdmin                                                 |
| Orchestration   | Docker & Docker Compose                                    |
| Build Backend   | Maven                                                      |
| Build Frontend  | Node / Angular CLI                                         |



## Containerisation avec Docker
 **1. Dockerfile Backend (Spring Boot)**


```text
FROM maven:3.9.6-eclipse-temurin-17 AS builder
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=builder /app/target/*.jar app.jar
EXPOSE 8085
ENTRYPOINT ["java","-jar","app.jar"]
```

<img width="779" height="514" alt="image" src="https://github.com/user-attachments/assets/daf58fa6-fa90-4c37-bdec-6854243e7a57" />

**2. Dockerfile Frontend (Angular)**


```text
FROM node:16-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist/smart-home /usr/share/nginx/html
EXPOSE 80
```

<img width="1600" height="759" alt="image" src="https://github.com/user-attachments/assets/f659e070-4532-4eb0-ad92-759f1188f6b5" />

## docker-compose.yml



```text
version: '3.8'

services:
  mysql:
    image: mysql:8.0
    container_name: mysql-smart-house
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: smart-house
    ports:
      - "3307:3306"
    command: --default-authentication-plugin=mysql_native_password
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost", "-proot"]
      interval: 5s
      timeout: 3s
      retries: 30
    volumes:
      - mysql_data:/var/lib/mysql
    restart: unless-stopped

  backend:
    build:
      context: ./Smart_Home_back
    container_name: backend-smart-house
    ports:
      - "8085:8085"
    depends_on:
      mysql:
        condition: service_healthy
    environment:
      SPRING_DATASOURCE_URL: jdbc:mysql://mysql:3306/smart-house
      SPRING_DATASOURCE_USERNAME: root
      SPRING_DATASOURCE_PASSWORD: root
    restart: unless-stopped

  frontend:
    build:
      context: ./smartHome-front
    container_name: frontend-smart-house
    ports:
      - "8080:80"
    depends_on:
      backend:
        condition: service_started
    restart: unless-stopped

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: phpmyadmin-smart-house
    environment:
      PMA_HOST: mysql
      PMA_PORT: 3306
      MYSQL_ROOT_PASSWORD: root
    ports:
      - "8082:80"
    depends_on:
      mysql:
        condition: service_healthy
    restart: unless-stopped

volumes:
  mysql_data:
```

<img width="1600" height="842" alt="image" src="https://github.com/user-attachments/assets/3346bae5-bcfc-4bd6-be49-d531e99a4b46" />

<img width="1426" height="266" alt="image" src="https://github.com/user-attachments/assets/964741c5-9912-45d8-809b-a877311eab0b" />
<img width="1600" height="263" alt="image" src="https://github.com/user-attachments/assets/869ad7f0-41fb-4cad-9472-2ba374736618" />


## Lancement du projet

**Assure-toi que Docker Desktop est lancé.**

 **1. Lancer tous les services**

 ```text
docker compose up -d
```

**2. Arrêter et nettoyer**


```text
docker compose down
```

**3. Rebuild complet**

```text
docker compose build
```

**Accès aux services**


| Service                 | URL                                            |
| ----------------------- | ---------------------------------------------- |
| **Frontend Angular**    | [http://localhost:8080](http://localhost:8080) |
| **Backend Spring Boot** | [http://localhost:8085](http://localhost:8085) |
| **phpMyAdmin**          | [http://localhost:8082](http://localhost:8082) |
| **MySQL (local)**       | hôte : `localhost`, port : `3307`              |



## Tests recommandés


**Ajouter une catégorie**

**Menu Categories → Add Category**

**Exemple :**

- **Label : Lumières intérieures**

- **Description : Éclairages dans la maison**

- **Ajouter un device**

- **Menu Devices → Add Device**

- **Remplir le formulaire**

- **Photo optionnelle**

- **Sélectionner la catégorie**

- **Cliquer Save**



https://github.com/user-attachments/assets/72555909-12b9-47ce-94cd-8b9ab1b3e273



## Auteur

**Projet réalisé par Kaoutar AITLBIZ**
**Encadré dans le cadre des projets Microservices & SmartHome.**
**Docker, Spring Boot, Angular, MySQL.**



