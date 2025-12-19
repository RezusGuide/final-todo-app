# 🚀 Final ToDo App - DevOps Exam Project

**Final ToDo App** — это полноценное веб-приложение для управления задачами, разработанное с применением передовых практик DevOps. Проект включает в себя исходный код на Java (Spring Boot), автоматизацию инфраструктуры (Ansible), контейнеризацию (Docker), оркестрацию (Kubernetes) и CI/CD пайплайн (Jenkins).

---

## 🛠 Технологический стек

* **Backend:** Java 17, Spring Boot 3, Gradle 8.5
* **Database:** PostgreSQL 15 (Alpine)
* **Containerization:** Docker (Multi-stage build)
* **Orchestration:** Kubernetes (Deployments, Services, HPA, PVC)
* **CI/CD:** Jenkins (Declarative Pipeline)
* **Infrastructure as Code:** Ansible
* **Version Control:** Git

---

## 📂 Структура проекта

```text
final-todo-app/
├── ansible/                 # Ansible для настройки инфраструктуры
│   ├── roles/               # Роли: common, docker, jenkins, kubernetes
│   ├── inventory-BatyrK.ini # Инвентарь хостов
│   └── playbook-BatyrK.yml  # Главный плейбук
├── app/                     # Исходный код приложения (Spring Boot)
│   ├── src/                 # Java классы и тесты
│   └── build.gradle         # Конфигурация сборки
├── docker/                  # Docker конфигурации
│   ├── Dockerfile-BatyrK    # Multi-stage Dockerfile
│   ├── docker-compose-BatyrK.yml
│   └── init-db.sql          # Скрипт инициализации БД
├── k8s/                     # Kubernetes манифесты
│   ├── deployment-BatyrK.yaml
│   ├── service-BatyrK.yaml
│   ├── configmap-BatyrK.yaml
│   ├── secret-BatyrK.yaml
│   └── hpa-BatyrK.yaml
├── Jenkinsfile-BatyrK       # CI/CD пайплайн
└── README.md                # Документация

Метод,URL,Описание
GET,/api/todos,Получить список всех задач
POST,/api/todos,Создать новую задачу
GET,/api/todos/{id},Получить задачу по ID
PUT,/api/todos/{id},Обновить задачу
DELETE,/api/todos/{id},Удалить задачу
GET,/api/todos/status/{completed},Фильтрация по статусу (true/false)
GET,/api/todos/search?title=...,Поиск задач по названию

Автоматизация инфраструктуры (Ansible)
Проект использует Ansible для полной настройки сервера "с нуля".

Что делает плейбук:

Common: Обновление кэша apt, базовая настройка ОС.

Docker: Установка Docker Engine, добавление пользователя в группу docker.

Jenkins: Установка Jenkins, Java 17, настройка репозитория и ожидание старта сервиса.

Kubernetes: Установка kubectl и инструментов для кластера (Minikube/Kind).

Запуск:

Bash

cd ansible
ansible-playbook -i inventory-BatyrK.ini playbook-BatyrK.yml --ask-vault-pass

CI/CD Пайплайн (Jenkins)
Пайплайн описан в файле Jenkinsfile-BatyrK и состоит из следующих этапов:

Checkout: Клонирование репозитория.

Build: Сборка JAR-файла через Gradle Wrapper.

Test: Запуск модульных тестов и публикация отчетов JUnit/HTML.

Code Analysis: Статический анализ кода (для веток main/develop).

Docker Build: Сборка образа и тегирование (версия билда + latest).

Docker Push: Авторизация и отправка образа в Docker Hub.

Deploy to Kubernetes: Обновление деплоймента в кластере (rolling update).

Запуск в Kubernetes
Для ручного развертывания приложения в кластере выполните следующие команды, используя манифесты из папки k8s/:

# 1. Конфигурация и секреты
kubectl apply -f k8s/configmap-BatyrK.yaml
kubectl apply -f k8s/secret-BatyrK.yaml

# 2. Хранилище и База данных
kubectl apply -f k8s/pvc-BatyrK.yaml
# (Postgres разворачивается как часть deployment-BatyrK.yaml или отдельно)

# 3. Приложение
kubectl apply -f k8s/deployment-BatyrK.yaml

# 4. Сервисы и Автомасштабирование
kubectl apply -f k8s/service-BatyrK.yaml
kubectl apply -f k8s/hpa-BatyrK.yaml

Доступ к приложению:

Внутри кластера: Порт 8080.

NodePort: Порт 30080.

Port-Forward (для тестов):

Bash

kubectl port-forward service/todo-app-service 8085:8080
Доступно по адресу: http://localhost:8085.

Запуск через Docker Compose
cd docker
docker-compose -f docker-compose-BatyrK.yml up --build

👨‍💻 Автор
Разработчик: BatyrK

Проект: Final DevOps Exam
