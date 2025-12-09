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


