# Домашнее задание к занятию «Что такое DevOps. СI/СD» `Старцев Данила Антонович`

### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` [репозитория c шаблоном решения](https://github.com/netology-code/sys-pattern-homework) к себе в GitHub и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw.
   2. Выполните клонирование этого репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите сверху название занятия, ваши фамилию и имя;
      - в каждом задании добавьте решение в требуемом виде — текст, код, скриншоты, ссылка.
      - для корректного добавления скриншотов используйте [инструкцию «Как вставить скриншот в шаблон с решением»](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md);
      - при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md).
   4. После завершения работы над домашним заданием сделайте коммит `git commit -m "comment"` и отправьте его на GitHub `git push origin`.
   5. Для проверки домашнего задания в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем GitHub.
   6. Любые вопросы по выполнению заданий задавайте в разделе «Вопросы по заданию» в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!

---

### Задание 1

**Что нужно сделать:**

1. Установите себе jenkins по инструкции из лекции или любым другим способом из официальной документации. Использовать Docker в этом задании нежелательно.
2. Установите на машину с jenkins [golang](https://golang.org/doc/install).
3. Используя свой аккаунт на GitHub, сделайте себе форк [репозитория](https://github.com/netology-code/sdvps-materials.git). В этом же репозитории находится [дополнительный материал для выполнения ДЗ](https://github.com/netology-code/sdvps-materials/blob/main/CICD/8.2-hw.md).
3. Создайте в jenkins Freestyle Project, подключите получившийся репозиторий к нему и произведите запуск тестов и сборку проекта ```go test .``` и  ```docker build .```.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

Скриншот-1 к заданию 1:
![скриншот 1](https://github.com/MindMaze74/devCICD/blob/main/img/1.png)

Скриншот-2 к заданию 1:
![скриншот 2](https://github.com/MindMaze74/devCICD/blob/main/img/2.png)

Скриншот-3 к заданию 1:
![скриншот 3](https://github.com/MindMaze74/devCICD/blob/main/img/3.png)

Скриншот-4 к заданию 1:
![скриншот 4](https://github.com/MindMaze74/devCICD/blob/main/img/4.png)

---

### Задание 2

**Что нужно сделать:**

1. Создайте новый проект pipeline.
2. Перепишите сборку из задания 1 на declarative в виде кода.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

Скриншот-5 к заданию 5:
![скриншот 5](https://github.com/MindMaze74/devCICD/blob/main/img/5.png)

```
pipeline {
    agent any
    stages {
        stage('Git') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/MindMaze74/sdvps-materials.git'
            }
        }
        stage('Test') {
            steps {
                sh 'ls -la' // покажет содержимое корня
                sh '/usr/local/go/bin/go test .'
            }
        }
        stage('Build') {
            steps {
                sh 'docker build . -t ubuntu-bionic:8082/hello-world:v$BUILD_NUMBER'
            }
        }
    }
}
```

Скриншот-6 к заданию 5:
![скриншот 6](https://github.com/MindMaze74/devCICD/blob/main/img/6.png)

Скриншот-7 к заданию 5:
![скриншот 7](https://github.com/MindMaze74/devCICD/blob/main/img/7.png)
---

### Задание 3

**Что нужно сделать:**

1. Установите на машину Nexus.
1. Создайте raw-hosted репозиторий.
1. Измените pipeline так, чтобы вместо Docker-образа собирался бинарный go-файл. Команду можно скопировать из Dockerfile.
1. Загрузите файл в репозиторий с помощью jenkins.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

Скриншот-8 к заданию 5:
![скриншот 8](https://github.com/MindMaze74/devCICD/blob/main/img/8.png)

Скриншот-9 к заданию 5:
![скриншот 9](https://github.com/MindMaze74/devCICD/blob/main/img/9.png)

Скриншот-10 к заданию 5:
![скриншот 10](https://github.com/MindMaze74/devCICD/blob/main/img/10.png)

Скриншот-11 к заданию 5:
![скриншот 11](https://github.com/MindMaze74/devCICD/blob/main/img/11.png)

Скриншот-12 к заданию 5:
![скриншот 12](https://github.com/MindMaze74/devCICD/blob/main/img/12.png)

Скриншот-13 к заданию 5:
![скриншот 13](https://github.com/MindMaze74/devCICD/blob/main/img/13.png)

```
pipeline {
    agent any
    
    environment {
        PATH = "/usr/local/go/bin:${env.PATH}"
    }
    
    stages {
        stage('Git') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/MindMaze74/sdvps-materials.git'
            }
        }
        
        stage('Build Binary') {
            steps {
                sh '''
                    
                    CGO_ENABLED=0 GOOS=linux go build -a -installsuffix nocgo -o app .
                '''
            }
        }
        
        stage('Upload to Nexus') {
            steps {
                sh '''
                    
                    curl -v --user admin:qwe1234 \
                        --upload-file app \
                       http://192.168.123.10:8081/repository/zadanie3/app-v${BUILD_NUMBER}
                '''
            }
        }
    }
}
```
---
## Дополнительные задания* (со звёздочкой)

Их выполнение необязательное и не влияет на получение зачёта по домашнему заданию. Можете их решить, если хотите лучше разобраться в материале.

---

### Задание 4*

Придумайте способ версионировать приложение, чтобы каждый следующий запуск сборки присваивал имени файла новую версию. Таким образом, в репозитории Nexus будет храниться история релизов.

Подсказка: используйте переменную BUILD_NUMBER.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.