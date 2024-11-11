# Домашнее задание к занятию «Запуск приложений в K8S»

### Цель задания

В тестовой среде для работы с Kubernetes, установленной в предыдущем ДЗ, необходимо развернуть Deployment с приложением, состоящим из нескольких контейнеров, и масштабировать его.

------

### Чеклист готовности к домашнему заданию

1. Установленное k8s-решение (например, MicroK8S).
2. Установленный локальный kubectl.
3. Редактор YAML-файлов с подключённым git-репозиторием.

------

### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. [Описание](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) Deployment и примеры манифестов.
2. [Описание](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) Init-контейнеров.
3. [Описание](https://github.com/wbitt/Network-MultiTool) Multitool.

------

### Задание 1. Создать Deployment и обеспечить доступ к репликам приложения из другого Pod

1. Создать Deployment приложения, состоящего из двух контейнеров — nginx и multitool. Решить возникшую ошибку.

<img width="578" alt="Create deployment and Error" src="https://github.com/user-attachments/assets/df89ca64-b6ed-4e64-8cb4-ed295bd4f595">

[deployment.yaml](https://github.com/sash3939/Kubernetes3-Start_App/blob/main/deployment.yaml)

По логам, проблема с bind портом. Решается через указание ENV. Добавил в yaml:

```yaml
        env:
        - name: HTTP_PORT
          value: "7080"
```

Затем **microk8s kubectl apply -f deployment.yaml**

<img width="521" alt="ENV  block added" src="https://github.com/user-attachments/assets/ab15ec72-06b6-43ba-a8ee-a574a95d9a67">

<img width="748" alt="curl 7080" src="https://github.com/user-attachments/assets/26decfe7-9376-4161-920c-5c936f5a1ee4">

2. После запуска увеличить количество реплик работающего приложения до 2.

<img width="445" alt="Replica" src="https://github.com/user-attachments/assets/14b6db26-c969-462d-9785-53dd48de8dc7">

3. Продемонстрировать количество подов до и после масштабирования.

Видно по времени создания

<img width="446" alt="Add replica 2" src="https://github.com/user-attachments/assets/53a69cbf-5c25-4030-bb90-470b68d4799b">

4. Создать Service, который обеспечит доступ до реплик приложений из п.1.

[deployment-svc.yaml](https://github.com/sash3939/Kubernetes3-Start_App/blob/main/deployment-svc.yaml)

Затем **kubectl apply -f deployment-svc.yaml**

<img width="446" alt="Service created" src="https://github.com/user-attachments/assets/3f75aa6c-4513-4a5a-9508-f5676af8fdd5">

<img width="475" alt="get ep" src="https://github.com/user-attachments/assets/55626784-5496-4cd7-82b7-a9617d508fad">

5. Создать отдельный Pod с приложением multitool и убедиться с помощью `curl`, что из пода есть доступ до приложений из п.1.

[multitool-app.yaml](https://github.com/sash3939/Kubernetes3-Start_App/blob/main/multitool-app.yaml)

Затем **microk8s kubectl apply -f multitool-app.yaml**

<img width="356" alt="Add pod for multitool" src="https://github.com/user-attachments/assets/5e0f0055-e002-4325-9874-916f78c91aeb">
---

<img width="520" alt="Access to app" src="https://github.com/user-attachments/assets/0a492f14-a0f7-477f-bffa-e1f197b093ec">

<img width="525" alt="Access to app1" src="https://github.com/user-attachments/assets/0b36574e-f54c-4c64-a0c0-0e8eaacb6db7">

<img width="748" alt="curl access app" src="https://github.com/user-attachments/assets/692f9e53-3f40-444d-b5ba-dee9572300a6">

------

### Задание 2. Создать Deployment и обеспечить старт основного контейнера при выполнении условий

1. Создать Deployment приложения nginx и обеспечить старт контейнера только после того, как будет запущен сервис этого приложения.
2. Убедиться, что nginx не стартует. В качестве Init-контейнера взять busybox.
3. Создать и запустить Service. Убедиться, что Init запустился.
4. Продемонстрировать состояние пода до и после запуска сервиса.

------

### Правила приема работы

1. Домашняя работа оформляется в своем Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl` и скриншоты результатов.
3. Репозиторий должен содержать файлы манифестов и ссылки на них в файле README.md.

------
