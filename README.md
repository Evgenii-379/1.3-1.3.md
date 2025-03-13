# **Домашнее задание к занятию «Запуск приложений в K8S»**-***Вуколов Евгений***
 
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
2. После запуска увеличить количество реплик работающего приложения до 2.
3. Продемонстрировать количество подов до и после масштабирования.
4. Создать Service, который обеспечит доступ до реплик приложений из п.1.
5. Создать отдельный Pod с приложением multitool и убедиться с помощью `curl`, что из пода есть доступ до приложений из п.1.
 
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


# **Решение**

## **Задание 1**

- Создаю Deployment приложения, состоящего из двух контейнеров — nginx и multitool. При запуске возникает ошибка, multitool не запускается, так как происходит конфликт портов, поэтому требуется 
в конфигурации файла указать альтернативный порт. Добавляю переменную с указанием порта 1180 

- ![scrin](https://github.com/Evgenii-379/1.3-1.3.md/blob/main/Снимок%20экрана%202025-03-13%20134725.png)

- Для выполнения 1-го задания создаю namespace под названием netology-task:

- ![scrin](https://github.com/Evgenii-379/1.3-1.3.md/blob/main/Снимок%20экрана%202025-03-13%20153500.png)

- Демонстрация количества подов до масштабирования и после: 

- ![scrin](https://github.com/Evgenii-379/1.3-1.3.md/blob/main/Снимок%20экрана%202025-03-13%20155838.png)
- ![scrin](https://github.com/Evgenii-379/1.3-1.3.md/blob/main/Снимок%20экрана%202025-03-13%20155948.png)

- Создание Service : 

- ![scrin](https://github.com/Evgenii-379/1.3-1.3.md/blob/main/Снимок%20экрана%202025-03-13%20161438.png)

- multitool-pod :
- ![scrin](https://github.com/Evgenii-379/1.3-1.3.md/blob/main/Снимок%20экрана%202025-03-13%20162242.png)

- С помощью curl, убеждаемся что доступ есть: 

- ![scrin](https://github.com/Evgenii-379/1.3-1.3.md/blob/main/Снимок%20экрана%202025-03-13%20170044.png)
- ![scrin](https://github.com/Evgenii-379/1.3-1.3.md/blob/main/Снимок%20экрана%202025-03-13%20170240.png)


## **Задание 2**

- Сначала создаю Deployment БЕЗ сервиса и вывожу статус pod :

- ![scrin](https://github.com/Evgenii-379/1.3-1.3.md/blob/main/Снимок%20экрана%202025-03-13%20230354.png)

- Как видно pod не запустился :
- ![scrin](https://github.com/Evgenii-379/1.3-1.3.md/blob/main/Снимок%20экрана%202025-03-13%20230446.png)

- Создаём сервис и снова вывожу статус pod, теперь работает : 

- ![scrin](https://github.com/Evgenii-379/1.3-1.3.md/blob/main/Снимок%20экрана%202025-03-13%20230741.png)
- ![scrin](https://github.com/Evgenii-379/1.3-1.3.md/blob/main/Снимок%20экрана%202025-03-13%20231207.png)

- Ссылки манифестов : 

[Service.yaml](https://github.com/Evgenii-379/1.3-1.3.md/blob/main/yaml_configs/Service.yaml)
[deployment.yaml](https://github.com/Evgenii-379/1.3-1.3.md/blob/main/yaml_configs/deployment.yaml)
[multitool-pod.yaml](https://github.com/Evgenii-379/1.3-1.3.md/blob/main/yaml_configs/multitool-pod.yaml)
[nginx-deployment.yaml](https://github.com/Evgenii-379/1.3-1.3.md/blob/main/yaml_configs/nginx-deployment.yaml)
[nginx-service.yml](https://github.com/Evgenii-379/1.3-1.3.md/blob/main/yaml_configs/nginx-service.yml)




















- ![scrin](https://github.com/
- ![scrin](https://github.com/
