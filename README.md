## Дополнительные материалы для выполнения домашних заданий из блока "Введение в DevOps"
Задание 1
Установите себе jenkins по инструкции из лекции или любым другим способом из официальной документации. Использовать Docker в этом задании нежелательно.
Установите на машину с jenkins golang.
Используя свой аккаунт на GitHub, сделайте себе форк репозитория. В этом же репозитории находится дополнительный материал для выполнения ДЗ.
Создайте в jenkins Freestyle Project, подключите получившийся репозиторий к нему и произведите запуск тестов и сборку проекта go test . и docker build ..
В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.
 
Решение 1
sudo apt update
sudo apt upgrade
sudo apt install jenkins
sudo systemctl start jenkins
sudo service jenkins status
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw allow 8080
sudo ufw status
wget https://go.dev/dl/go1.22.1.linux-amd64.tar.gz
sha256sum go1.22.1.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.22.1.linux-amd64.tar.gz
sudo nano .profile
Добавляем в конце:
export PATH=$PATH:/usr/local/go/bin
export GOPATH=$HOME/goproject
export PATH=$PATH:$GOPATH/bin


source ~/.profile
mkdir $HOME/goproject
go version
sudo su
cat /var/lib/jenkins/secrets/initialAdminPassword
sudo apt-get install -y apt-transport-https ca-certificates curl
curl -fsSL https://get.docker.com -o get-docker.sh
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo usermod -aG docker jenkins
sudo su
sudo service jenkins stop
sudo service jenkins start

sudo mkdir myrepo
cd myrepo/
sudo git clone https://github.com/StasAlginin/8-03-hw.git

Выполнение команды shell:
/usr/local/go/bin/go test .
docker build . -t ubuntu-bionic:8082/hello-world:v$BUILD_NUMBER

Выполнение:
Started by user algininss
Running as SYSTEM
Building in workspace /var/lib/jenkins/workspace/Freestyle Project
The recommended git tool is: NONE
No credentials specified
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/Freestyle Project/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/StasAlginin/8-03-hw.git # timeout=10
Fetching upstream changes from https://github.com/StasAlginin/8-03-hw.git
 > git --version # timeout=10
 > git --version # 'git version 2.17.1'
 > git fetch --tags --progress -- https://github.com/StasAlginin/8-03-hw.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/main^{commit} # timeout=10
Checking out Revision 223dbc3f489784448004e020f2ef224f17a7b06d (refs/remotes/origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 223dbc3f489784448004e020f2ef224f17a7b06d # timeout=10
Commit message: "Update README.md"
 > git rev-list --no-walk 223dbc3f489784448004e020f2ef224f17a7b06d # timeout=10
[Freestyle Project] $ /bin/sh -xe /tmp/jenkins16587756053986731467.sh
+ /usr/local/go/bin/go test .
ok  	github.com/netology-code/sdvps-materials	(cached)
+ docker build . -t ubuntu-bionic:8082/hello-world:v5

1 [internal] load build definition from Dockerfile
1 transferring dockerfile: 350B done
1 DONE 0.0s

2 [internal] load .dockerignore
2 transferring context: 2B done
2 DONE 0.0s

3 [internal] load metadata for docker.io/library/golang:1.16
3 DONE 1.1s

4 [internal] load metadata for docker.io/library/alpine:latest
4 DONE 1.3s

5 [builder 1/4] FROM docker.io/library/golang:1.16@sha256:5f6a4662de3efc6d6bb812d02e9de3d8698eea16b8eb7281f03e6f3e8383018e
5 DONE 0.0s

6 [stage-1 1/3] FROM docker.io/library/alpine:latest@sha256:c5b1261d6d3e43071626931fc004f70149baeba2c8ec672bd4f27761f8e1ad6b
6 DONE 0.0s

7 [internal] load build context
7 transferring context: 13.06kB done
7 DONE 0.0s

8 [builder 3/4] COPY . ./
8 CACHED

9 [stage-1 2/3] RUN apk -U add ca-certificates
9 CACHED

10 [builder 4/4] RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix nocgo -o /app .
10 CACHED

11 [builder 2/4] WORKDIR /go/src/github.com/netology-code/sdvps-materials
11 CACHED

12 [stage-1 3/3] COPY --from=builder /app /app
12 CACHED

13 exporting to image
13 exporting layers done
13 writing image sha256:3e7767b24c190753213069285af0238f727ea07485a3d1b2d65de6de3b3a2c77 done
13 naming to ubuntu-bionic:8082/hello-world:v5 done
13 DONE 0.0s
Finished: SUCCESS

Задание 2
Создайте новый проект pipeline.
Перепишите сборку из задания 1 на declarative в виде кода.
В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.


Решение 2

pipeline {
 agent any
 stages {
  stage('Git') {
   steps {git 'https://github.com/StasAlginin/8-03-hw.git'}
  }
  stage('Test') {
   steps {
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

Выполнение:
Started by user algininss

[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins
in /var/lib/jenkins/workspace/newpipeline
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Git)
[Pipeline] git
The recommended git tool is: NONE
No credentials specified
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/newpipeline/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/StasAlginin/8-03-hw.git # timeout=10
Fetching upstream changes from https://github.com/StasAlginin/8-03-hw.git
 > git --version # timeout=10
 > git --version # 'git version 2.17.1'
 > git fetch --tags --progress -- https://github.com/StasAlginin/8-03-hw.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
Checking out Revision da5acf7bcb7f437637adf06fbd03a24dc2c8f13e (refs/remotes/origin/master)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f da5acf7bcb7f437637adf06fbd03a24dc2c8f13e # timeout=10
 > git branch -a -v --no-abbrev # timeout=10
 > git branch -D master # timeout=10
 > git checkout -b master da5acf7bcb7f437637adf06fbd03a24dc2c8f13e # timeout=10
Commit message: "branch main, add creds for vagrant box"
 > git rev-list --no-walk da5acf7bcb7f437637adf06fbd03a24dc2c8f13e # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Test)
[Pipeline] sh
+ /usr/local/go/bin/go test .
ok  	github.com/netology-code/sdvps-materials	0.001s
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Build)
[Pipeline] sh
+ docker build . -t ubuntu-bionic:8082/hello-world:v9
1 [internal] load .dockerignore
1 transferring context: 2B done
1 DONE 0.1s

2 [internal] load build definition from Dockerfile
2 transferring dockerfile: 350B done
2 DONE 0.1s

3 [internal] load metadata for docker.io/library/golang:1.16
3 DONE 1.4s

4 [internal] load metadata for docker.io/library/alpine:latest
4 DONE 1.5s

5 [stage-1 1/3] FROM docker.io/library/alpine:latest@sha256:c5b1261d6d3e43071626931fc004f70149baeba2c8ec672bd4f27761f8e1ad6b
5 DONE 0.0s

6 [builder 1/4] FROM docker.io/library/golang:1.16@sha256:5f6a4662de3efc6d6bb812d02e9de3d8698eea16b8eb7281f03e6f3e8383018e
6 DONE 0.0s

7 [internal] load build context
7 transferring context: 94.48kB 0.6s done
7 DONE 0.6s

8 [builder 2/4] WORKDIR /go/src/github.com/netology-code/sdvps-materials
8 CACHED

9 [builder 3/4] COPY . ./
9 DONE 1.3s

10 [builder 4/4] RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix nocgo -o /app .
10 DONE 21.4s

11 [stage-1 2/3] RUN apk -U add ca-certificates
11 CACHED

12 [stage-1 3/3] COPY --from=builder /app /app
12 CACHED

13 exporting to image
13 exporting layers done
13 writing image sha256:3e7767b24c190753213069285af0238f727ea07485a3d1b2d65de6de3b3a2c77 done
13 naming to ubuntu-bionic:8082/hello-world:v9 0.0s done
13 DONE 0.0s
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS

Задание 3
Установите на машину Nexus.
Создайте raw-hosted репозиторий.
Измените pipeline так, чтобы вместо Docker-образа собирался бинарный go-файл. Команду можно скопировать из Dockerfile.
Загрузите файл в репозиторий с помощью jenkins.
В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

Решение 3
Разворачиваем с помощью Docker nexus
docker run -d -p 10.128.0.32:8081:8081 -p 10.128.0.32:8082:8082 --name nexus -e INSTALL4J_ADD_VM_PARAMS="-Xms512m -Xmx512m -XX:MaxDirectMemorySize=273m -Djava.util.prefs.userRoot=${NEXUS_DATA}/javaprefs" sonatype/nexus3
Переходим на сайт

Выводим пароль для авторизации:
docker exec -t nexus bash -c 'cat /nexus-data/admin.password && echo'
Выводится пароль для 1 авторизации 
Логин: admin
Пароль: d78fda77-2a2a-4db3-8c1c-cee8652edf38
Авторизируемся в nexus и меняем пароль

И создаем raw (hosted) репозиторий

В командной строке используем команду:
cd /usr/local/ && echo "export PATH=\$PATH:/usr/local/go/bin:\$HOME/go/bin" >> ~/.bashrc && echo "export GOROOT=/usr/local/go" >> ~/.bashrc && echo "export PATH=\$PATH:/usr/local/go/bin:\$HOME/go/bin" >> /home/algininss/.bashrc && echo "export GOROOT=/usr/local/go" >> /home/algininss/.bashrc && source ~/.bashrc && source /home/algininss/.bashrc
И добавляем в файл  sudo nano ~/.profile в конце следующие строчки:
export PATH=$PATH:/usr/local/go/bin
export GOPATH=$HOME/goproject
export PATH=$PATH:$GOPATH/bin
export GOPATH=$HOME/go
export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin


Изменяем pipeline:
pipeline {
    agent any
    stages {
        stage('Git') {
            steps {
                git 'https://github.com/StasAlginin/8-03-hw.git'
            }
        }
        
        stage('create') {
            steps {
                script {
                    withEnv(["PATH+GO=/usr/local/go/bin"]) {
                        sh 'go build -a -buildvcs=false -installsuffix nocgo -o /home/algininss/myrepo/8-03-hw/app .'
                    }
                }
            }
        }
        
        stage('Push') {
            steps {
                sh 'curl -v -u admin:123 --upload-file /home/algininss/myrepo/8-03-hw/app http://158.160.114.119:8081/repository/myrepo/app/'
            }
        }
    }
}
 
Вывод:
Started by user algininss


[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins
in /var/lib/jenkins/workspace/newpipeline
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Git)
[Pipeline] git
The recommended git tool is: NONE
No credentials specified
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/newpipeline/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/StasAlginin/8-03-hw.git # timeout=10
Fetching upstream changes from https://github.com/StasAlginin/8-03-hw.git
 > git --version # timeout=10
 > git --version # 'git version 2.17.1'
 > git fetch --tags --progress -- https://github.com/StasAlginin/8-03-hw.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
Checking out Revision da5acf7bcb7f437637adf06fbd03a24dc2c8f13e (refs/remotes/origin/master)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f da5acf7bcb7f437637adf06fbd03a24dc2c8f13e # timeout=10
 > git branch -a -v --no-abbrev # timeout=10
 > git branch -D master # timeout=10
 > git checkout -b master da5acf7bcb7f437637adf06fbd03a24dc2c8f13e # timeout=10
Commit message: "branch main, add creds for vagrant box"
 > git rev-list --no-walk da5acf7bcb7f437637adf06fbd03a24dc2c8f13e # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (create)
[Pipeline] script
[Pipeline] {
[Pipeline] withEnv
[Pipeline] {
[Pipeline] sh
+ go build -a -buildvcs=false -installsuffix nocgo -o /home/algininss/myrepo/8-03-hw/app .
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Push)
[Pipeline] sh
+ curl -v -u admin:123 --upload-file /home/algininss/myrepo/8-03-hw/app http://158.160.114.119:8081/repository/myrepo/app/
*   Trying 158.160.114.119...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed


  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Connected to 158.160.114.119 (158.160.114.119) port 8081 (#0)
* Server auth using Basic with user 'admin'
> PUT /repository/myrepo/app/app HTTP/1.1
> Host: 158.160.114.119:8081
> Authorization: Basic YWRtaW46MTIz
> User-Agent: curl/7.58.0
> Accept: */*
> Transfer-Encoding: chunked
> Expect: 100-continue
> 
< HTTP/1.1 100 Continue
* Signaling end of chunked upload via terminating chunk.
} [5 bytes data]
< HTTP/1.1 201 Created
< Date: Wed, 17 Apr 2024 18:38:21 GMT
< Server: Nexus/3.66.0-02 (OSS)
< X-Content-Type-Options: nosniff
< Content-Security-Policy: sandbox allow-forms allow-modals allow-popups allow-presentation allow-scripts allow-top-navigation
< X-XSS-Protection: 1; mode=block
< Content-Length: 0
< 


100     5    0     0    0     5      0      4 --:--:--  0:00:01 --:--:--     4
100     5    0     0    0     5      0      4 --:--:--  0:00:01 --:--:--     4
* Connection #0 to host 158.160.114.119 left intact
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS

- [Дополнительный материал для занятия "8.2. Что такое DevOps. СI/СD"](CICD/8.2-hw.md)

- [Дополнительный материал для занятия "8.3. GitLab"](https://github.com/netology-code/sdvps-materials/tree/main/gitlab)
