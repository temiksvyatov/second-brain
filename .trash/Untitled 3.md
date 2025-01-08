Привет, мне поставили следующую задачу:
"Нужно настроить CI на репозиторий для работы с БД Oracle

Проект на node js + express. Используется typescript. Работать с нашим Postgres сервис не будет, но будет подключаться к oracle базе

В переменных окружения добавить только:

environment=development

Сервис не должен быть доступен снаружи. Он должен быть доступен только внутри сети докера - подключаем к той же сети что и бэкенд ЦУВ

Используется порт 3000, но можно перепределить через переменные окружения.

При настройке сразу добавить dockerignore и сборку с актуальной версией nodejs (20.17.0)."
Dockerignore  у меня уже есть, это пропускаем. Что я у тебя хочу попросить: проверь все имеющиеся файлы для ci, которые у меня есть, и скажи, как их исправить для выполнения данной задачи (примечание: переменные окружения передаются через Gitlab CI Variables как файл APP_ENV, и он же передается в playbook, так что нигде в код добавлять environment не надо):
```
# .gitlab-ci.yml
image: docker/compose:debian-1.28.0-rc1

variables:
  DOCKER_BUILDKIT: '1'
  IMAGE_TAG: $GLOBAL_REGISTRY/$CI_PROJECT_PATH_SLUG:$CI_COMMIT_REF_SLUG-$CI_PIPELINE_IID

stages:
  - lint
  - build
  - deploy

# ANCHORS

.dev: &development_configuration
  environment: development
  tags:
    - vvdev
  only:
    - develop
  variables:
    VARS_FILE: vars/development.yml
    INVENTORY: deploy/ansible/inventory/development

# TEMPLATES

.build:
  stage: build
  before_script:
    - docker login --username oauth --password $GLOBAL_DOCKER_OAUTH cr.yandex
  script:
    - docker build -t $IMAGE_TAG -f deploy/build.dockerfile .
  after_script:
    - docker push $IMAGE_TAG

.deploy:
  stage: deploy
  image: volgavolga/ansible:1.0.0
  script:
    - echo "$GLOBAL_RUNNER_SSH_KEY" > deploy_rsa
    - chmod 400 deploy_rsa
    - ansible-playbook deploy/ansible/main.yml
      -i $INVENTORY
      --private-key deploy_rsa
      --extra-vars "APP_ENV=\"$APP_ENV\""
      --extra-vars "VARS_FILE=$VARS_FILE"
      --extra-vars "IMAGE_TAG=$IMAGE_TAG"

# DEVELOPMENT

lint_development:
  stage: lint
  <<: *development_configuration
  script:
    - docker build -f deploy/lint.dockerfile .

build_development:
  extends: .build
  <<: *development_configuration

deploy_development:
  extends: .deploy
  <<: *development_configuration
```
```
# build.dockerfile
# syntax = edrevo/dockerfile-plus
INCLUDE+ deploy/_dependencies.dockerfile

# Build
FROM node:18.17.1-buster as build
WORKDIR /opt
COPY --from=dependencies /opt ./
RUN yarn build
RUN echo "{ \"date\": \"$(date +'%Y-%m-%d %H:%M')\", \"version\": \"$(node -p "require('./package.json').version")\" }" > release-info.json

# Deploy
FROM node:18.17.1-buster AS deploy

RUN chmod 777 /opt
RUN useradd -m docker-node

COPY --from=build /opt/dist/ /opt/dist/
COPY --from=build /opt/node_modules/ /opt/node_modules/

COPY --from=build /opt/package.json /opt/package.json
COPY --from=build /opt/release-info.json /opt/release-info.json
COPY --from=build /opt/public/ /opt/public/


USER docker-node
WORKDIR /opt

CMD npm run start:prod
```
```
# lint.dockerfile
# syntax = edrevo/dockerfile-plus
INCLUDE+ deploy/_dependencies.dockerfile

FROM node:18.17.1-buster as linting
WORKDIR /opt
COPY --from=dependencies /opt ./
RUN yarn lint && yarn type-check
```
```
# _dependencies.dockerfile
FROM node:18.17.1-buster AS dependencies
WORKDIR /opt

COPY package.json .yarnr[c] yarn.loc[k] ./
RUN yarn --frozen-lockfile

COPY . .
```
```
# main.yml
---
- name: Deploy app
  hosts: cuv
  gather_facts: no
  vars_files:
    - "{{ VARS_FILE }}"
  tasks:
    - name: Create a network
      docker_network:
        name: "{{ NETWORK_NAME }}"

    - name: Create tmp directory
      ansible.builtin.file:
        path:  "{{ TEMPORARY_PROJECT_DIRECTORY }}"
        state: directory

    - name: Move template
      template:
        src: docker-compose.j2
        dest: "{{ TEMPORARY_PROJECT_DIRECTORY }}/docker-compose.yml"

    - name: Copy env_file from local to remote
      ansible.builtin.copy:
        src: '{{ APP_ENV }}'
        dest: "{{ TEMPORARY_PROJECT_DIRECTORY }}/.env"

    - name: Run migrations
      docker_compose:
        project_src: "{{ TEMPORARY_PROJECT_DIRECTORY }}"
        services:
          - migrations

    - name: Deploy application container
      docker_compose:
        project_src: "{{ TEMPORARY_PROJECT_DIRECTORY }}"
        services:
          - api

    - name: Remove tmp
      ansible.builtin.file:
        path: "{{ TEMPORARY_PROJECT_DIRECTORY }}"
        state: absent
```
```
# vars file development.yml
CONTAINER_PORT: 3166
NETWORK_NAME: development_cuv
CONTAINER_NAME: development_cuv_back
TEMPORARY_PROJECT_DIRECTORY: ./cuv_back
NGINX_SERVER_NAME: cuv.dev.vvdev.ru
```
```
# docker-compose.j2
version: '3.9'

services:
  migrations:
    image: {{ IMAGE_TAG }}
    container_name: "{{ CONTAINER_NAME }}_migrations"
    command: yarn run typeorm migration:run
    networks:
      - {{ NETWORK_NAME }}
    env_file:
      - .env

  api:
    restart: always
    image: {{ IMAGE_TAG }}
    container_name: {{ CONTAINER_NAME }}
    ports:
      - "{{ CONTAINER_PORT }}:8888"
    networks:
      - {{ NETWORK_NAME }}
    env_file:
      - .env
networks:
  {{ NETWORK_NAME }}:
    external: true
```
