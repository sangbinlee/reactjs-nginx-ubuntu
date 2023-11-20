# reactjs-nginx-ubuntu
reactjs-nginx-ubuntu

![image](https://github.com/sangbinlee/reactjs-nginx-ubuntu/assets/4024414/9ef0dcae-62ae-4b2a-9184-decfabd0c593)


# vi /etc/nginx/sites-available/sodi9.store
    
    sangbinlee9@master:~/front-end/react-1$ sudo vi /etc/nginx/sites-available/sodi9.store
    server {
    
            server_name sodi9.store www.sodi9.store;
    
            location / {
                    proxy_pass http://127.0.0.1:3000;
            }
    
    
        listen [::]:443 ssl ipv6only=on; # managed by Certbot
        listen 443 ssl; # managed by Certbot
        ssl_certificate /etc/letsencrypt/live/sodi9.store/fullchain.pem; # managed by Certbot
        ssl_certificate_key /etc/letsencrypt/live/sodi9.store/privkey.pem; # managed by Certbot
        include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
    
    
    }
    
    server {
        if ($host = www.sodi9.store) {
            return 301 https://$host$request_uri;
        } # managed by Certbot
    
    
        if ($host = sodi9.store) {
            return 301 https://$host$request_uri;
        } # managed by Certbot
    
    
    
            listen 80;
            listen [::]:80;
    
            server_name sodi9.store www.sodi9.store;
        return 404; # managed by Certbot
    
    
    
    
    }
    ~
    ~
    ~

#
#
# react-dockerizing/Dockerfile
    
    # base image 설정(as build 로 완료된 파일을 밑에서 사용할 수 있다.)
    FROM node:18-alpine as build
    
    # ENV NODE_ENV=production
    
    # 컨테이너 내부 작업 디렉토리 설정
    # RUN mkdir /app
    WORKDIR /app
    
    ENV PATH /app/node_modules/.bin:$PATH
    
    COPY package*.json ./
    
    # package.json 및 package-lock.json 파일에 명시된 의존성 패키지들을 설치
    RUN npm install --silent
    
    # 호스트 머신의 현재 디렉토리 파일들을 컨테이너 내부로 전부 복사
    # COPY . .    
    COPY . /app
    
    
    # 컨테이너의 80번 포트를 열어준다.
    # EXPOSE 80
    EXPOSE 3000
    
    # nginx 서버를 실행하고 백그라운드로 동작하도록 한다.
    # CMD ["nginx", "-g", "daemon off;"]
    CMD ["npm", "start"]
    
    
    
    ############################
    
    # 그리고 Dockerfile로 docker 이미지를 빌드해야한다.
    # $ docker build -t react-1:latest .
    
    
    ############################
    # docker run -it -p 3000:3000 react-1
    


# sudo docker ps -a

    sangbinlee9@master:~/front-end/react-1$ sudo docker ps -a
    [sudo] password for sangbinlee9:
    CONTAINER ID   IMAGE                                        COMMAND                  CREATED          STATUS                      PORTS                                                                                      NAMES
    fef0bf99de4e   react-1                                      "docker-entrypoint.s…"   57 minutes ago   Up 57 minutes               0.0.0.0:3000->3000/tcp, :::3000->3000/tcp                                                  trusting_mayer











#
#   docker stop fef0bf99de4e

        sangbinlee9@master:/etc/nginx/conf.d$ docker stop fef0bf99de4e
        permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/fef0bf99de4e/stop": dial unix /var/run/docker.sock: connect: permission denied
        sangbinlee9@master:/etc/nginx/conf.d$ sudo docker stop fef0bf99de4e
        [sudo] password for sangbinlee9:
        fef0bf99de4e
        sangbinlee9@master:/etc/nginx/conf.d$ sudo docker ps -a
        CONTAINER ID   IMAGE                                        COMMAND                  CREATED        STATUS                       PORTS                                                                                      NAMES
        fef0bf99de4e   react-1                                      "docker-entrypoint.s…"   3 hours ago    Exited (137) 6 seconds ago                                                                                              trusting_mayer

#
#  Dockerfile
        # react-dockerizing/Dockerfile
        
        # base image 설정(as builder 로 완료된 파일을 밑에서 사용할 수 있다.)
        FROM node:18-alpine as builder
        
        # ENV NODE_ENV=production
        
        # 컨테이너 내부 작업 디렉토리 설정
        # RUN mkdir /app
        WORKDIR /app
        
        ENV PATH /app/node_modules/.bin:$PATH
        
        COPY package*.json ./
        
        # package.json 및 package-lock.json 파일에 명시된 의존성 패키지들을 설치
        RUN npm install --silent
        
        # 호스트 머신의 현재 디렉토리 파일들을 컨테이너 내부로 전부 복사
        # COPY . .    
        COPY . /app
        
        
        ############################
        RUN npm run build
        
        
        
        
        
        ############################
        FROM nginx:stable-alpine
        # nginx의 기본 설정을 삭제하고 앱에서 설정한 파일을 복사
        RUN rm -rf /etc/nginx/conf.d
        COPY conf /etc/nginx
        
        # 위에서 생성한 앱의 빌드산출물을 nginx의 샘플 앱이 사용하던 폴더로 이동
        COPY --from=builder /app/build /usr/share/nginx/html
        
        # 80포트 오픈하고 nginx 실행
        EXPOSE 80
        CMD ["nginx", "-g", "daemon off;"]
        
         
        
        ############################
        
        # 그리고 Dockerfile로 docker 이미지를 빌드해야한다.
        # $ docker build -f Dockerfile-80 -t react-1:80 .
        
        
        ############################
        # docker run -it -p 80:80 react-1:80
        
        
        # $ docker build -f Dockerfile-80 -t react-1:80 .
        
        
        ############################
        # docker run -it -p 80:80 react-1:80
        






# Dockerfile-80
# Dockerfile-80
        
        sangbinlee9@master:~/front-end/react-1$ ll
        total 768
        drwxrwxr-x   6 sangbinlee9 sangbinlee9   4096 Nov 20 22:46 ./
        drwxrwxr-x   3 sangbinlee9 sangbinlee9   4096 Nov 20 16:59 ../
        drwxrwxr-x   3 sangbinlee9 sangbinlee9   4096 Nov 20 22:46 conf/
        -rw-rw-r--   1 sangbinlee9 sangbinlee9    963 Nov 20 22:46 Dockerfile
        -rw-rw-r--   1 sangbinlee9 sangbinlee9   1247 Nov 20 22:46 Dockerfile-80
        -rw-rw-r--   1 sangbinlee9 sangbinlee9    310 Nov 20 17:00 .gitignore
        drwxrwxr-x 854 sangbinlee9 sangbinlee9  36864 Nov 20 19:32 node_modules/
        -rw-rw-r--   1 sangbinlee9 sangbinlee9    810 Nov 20 17:00 package.json
        -rw-rw-r--   1 sangbinlee9 sangbinlee9 701497 Nov 20 17:00 package-lock.json
        drwxrwxr-x   2 sangbinlee9 sangbinlee9   4096 Nov 20 17:00 public/
        -rw-rw-r--   1 sangbinlee9 sangbinlee9   3359 Nov 20 17:00 README.md
        drwxrwxr-x   2 sangbinlee9 sangbinlee9   4096 Nov 20 17:00 src/

        
        sangbinlee9@master:~/front-end/react-1$  docker build -f Dockerfile-80 -t react-1:80 .

#
# 
        sangbinlee9@master:~/front-end/react-1$ sudo  docker build -f Dockerfile-80 -t react-1:80 .
        [+] Building 21.9s (16/16) FINISHED
         => [internal] load build definition from Dockerfile-80
         => => transferring dockerfile: 1.29kB
         => [internal] load .dockerignore
         => => transferring context: 2B
         => [internal] load metadata for docker.io/library/nginx:stable-alpine
         => [internal] load metadata for docker.io/library/node:18-alpine
         => [builder 1/6] FROM docker.io/library/node:18-alpine@sha256:3428c2de886bf4378657da6fe86e105573a609c94df1f7d6a70e57d2b51de21f
         => [stage-1 1/4] FROM docker.io/library/nginx:stable-alpine@sha256:62cabd934cbeae6195e986831e4f745ee1646c1738dbd609b1368d38c10c5519
         => => resolve docker.io/library/nginx:stable-alpine@sha256:62cabd934cbeae6195e986831e4f745ee1646c1738dbd609b1368d38c10c5519
         => => sha256:62cabd934cbeae6195e986831e4f745ee1646c1738dbd609b1368d38c10c5519 1.65kB / 1.65kB
         => => sha256:a4896b78e8db1b60c583e7bf115527dea0f1cc0207047cc318e6bab726a44765 3.85MB / 3.85MB
         => => sha256:a352ab20253014461883d177aacee4ee6a2f82b3976d65015665bf4e0c05478a 625B / 625B
         => => sha256:e06ffa63074d69b12c7f7712d7aebc4cc72d25217e55788d54160a2bd77182f4 1.78kB / 1.78kB
         => => sha256:e6295d4bbc4559ee7ed2e93830f4228a08af4114d7914db140a026f84e69adbb 16.32kB / 16.32kB
         => => sha256:9398808236ffac29e60c04ec906d8d409af7fa19dc57d8c65ad167e9c4967006 3.38MB / 3.38MB
         => => sha256:b9258afd06398fe15821fa84d20d293de602601e8f3c3f607eb0fb82077933a1 956B / 956B
         => => sha256:8799ab366479fb72b00d08daefe94af71814b80c946ba8a3a3262129f4ab6688 773B / 773B
         => => extracting sha256:9398808236ffac29e60c04ec906d8d409af7fa19dc57d8c65ad167e9c4967006
         => => sha256:07bc104f8702005e2c6acc3f3932ce08d11442367a62fd133a67157e7e5a8e6e 1.40kB / 1.40kB
         => => sha256:8afc9a751a90c1bc306e89a586c8e7b759a620814cb85a3584bd8c7833b4e9e4 11.67MB / 11.67MB
         => => extracting sha256:a4896b78e8db1b60c583e7bf115527dea0f1cc0207047cc318e6bab726a44765
         => => extracting sha256:a352ab20253014461883d177aacee4ee6a2f82b3976d65015665bf4e0c05478a
         => => extracting sha256:b9258afd06398fe15821fa84d20d293de602601e8f3c3f607eb0fb82077933a1
         => => extracting sha256:8799ab366479fb72b00d08daefe94af71814b80c946ba8a3a3262129f4ab6688
         => => extracting sha256:07bc104f8702005e2c6acc3f3932ce08d11442367a62fd133a67157e7e5a8e6e
         => => extracting sha256:8afc9a751a90c1bc306e89a586c8e7b759a620814cb85a3584bd8c7833b4e9e4
         => [internal] load build context
         => => transferring context: 3.32MB
         => CACHED [builder 2/6] WORKDIR /app
         => CACHED [builder 3/6] COPY package*.json ./
         => CACHED [builder 4/6] RUN npm install --silent
         => [builder 5/6] COPY . /app
         => [stage-1 2/4] RUN rm -rf /etc/nginx/conf.d
         => [stage-1 3/4] COPY conf /etc/nginx
         => [builder 6/6] RUN npm run build
         => [stage-1 4/4] COPY --from=builder /app/build /usr/share/nginx/html
         => exporting to image
         => => exporting layers
         => => writing image sha256:eba9cea9ba7aa93ebca329d730738ae62af611c9093f3d9e0c3205fb08b05f0a
         => => naming to docker.io/library/react-1:80

#
# sudo docker run -it -p 80:80 react-1:80
        
        sangbinlee9@master:~/front-end/react-1$ sudo docker run -it -p 80:80 react-1:80
        docker: Error response from daemon: driver failed programming external connectivity on endpoint amazing_borg (196ca17b3cbb4ef72b76a3912d9941b1fc8a206f92
        ERRO[0000] error waiting for container:


#
# systemctl status nginx

        
        sangbinlee9@master:~/front-end/react-1$ systemctl status nginx
        ● nginx.service - A high performance web server and a reverse proxy server
             Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
             Active: active (running) since Mon 2023-11-20 13:19:27 KST; 11h ago
               Docs: man:nginx(8)
           Main PID: 836 (nginx)
              Tasks: 5 (limit: 18901)
             Memory: 13.5M
                CPU: 304ms
             CGroup: /system.slice/nginx.service
                     ├─836 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
                     ├─837 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
                     ├─838 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
                     ├─839 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
                     └─840 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""


            
            Nov 20 13:19:26 master systemd[1]: Starting A high performance web server and a reverse proxy server...
            Nov 20 13:19:27 master systemd[1]: Started A high performance web server and a reverse proxy server.


#
# sudo systemctl stop nginx

            
            sangbinlee9@master:~/front-end/react-1$ sudo systemctl stop nginx
            sangbinlee9@master:~/front-end/react-1$ systemctl status nginx
            ○ nginx.service - A high performance web server and a reverse proxy server
                 Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
                 Active: inactive (dead) since Tue 2023-11-21 00:42:34 KST; 3s ago
                   Docs: man:nginx(8)
                Process: 21494 ExecStop=/sbin/start-stop-daemon --quiet --stop --retry QUIT/5 --pidfile /run/nginx.pid (code=exited, status=0/SUCCESS)
               Main PID: 836 (code=exited, status=0/SUCCESS)
                    CPU: 318ms
            
            Nov 20 13:19:26 master systemd[1]: Starting A high performance web server and a reverse proxy server...
            Nov 20 13:19:27 master systemd[1]: Started A high performance web server and a reverse proxy server.
            Nov 21 00:42:34 master systemd[1]: Stopping A high performance web server and a reverse proxy server...
            Nov 21 00:42:34 master systemd[1]: nginx.service: Deactivated successfully.
            Nov 21 00:42:34 master systemd[1]: Stopped A high performance web server and a reverse proxy server.


#
# sudo docker run -it -p 80:80 react-1:80
        
        sangbinlee9@master:~/front-end/react-1$ sudo docker run -it -p 80:80 react-1:80
        /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
        /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
        /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
        10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
        10-listen-on-ipv6-by-default.sh: info: /etc/nginx/conf.d/default.conf differs from the packaged version
        /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
        /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
        /docker-entrypoint.sh: Configuration complete; ready for start up
        2023/11/20 15:42:51 [notice] 1#1: using the "epoll" event method
        2023/11/20 15:42:51 [notice] 1#1: nginx/1.24.0
        2023/11/20 15:42:51 [notice] 1#1: built by gcc 12.2.1 20220924 (Alpine 12.2.1_git20220924-r4)
        2023/11/20 15:42:51 [notice] 1#1: OS: Linux 5.15.0-88-generic
        2023/11/20 15:42:51 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
        2023/11/20 15:42:51 [notice] 1#1: start worker processes
        2023/11/20 15:42:51 [notice] 1#1: start worker process 29
        2023/11/20 15:42:51 [notice] 1#1: start worker process 30

#
#

![image](https://github.com/sangbinlee/reactjs-nginx-ubuntu/assets/4024414/ab90778d-cdcf-44f5-b96e-555b75fb9af3)

![image](https://github.com/sangbinlee/reactjs-nginx-ubuntu/assets/4024414/efe3a993-e264-4665-a520-32dd6a4fed8e)

![image](https://github.com/sangbinlee/reactjs-nginx-ubuntu/assets/4024414/3daaaa4b-cc63-4b09-acd8-eac888cbc93f)



#
#

#
#

#
#

#
#

#
#

#
#

#
#

#
#

#
#

#
#

#
#

#
#

#
#

#
#

#
#

#
#

#
#

#
#

#
#

#
#

#
#

#
#
![image](https://github.com/sangbinlee/reactjs-nginx-ubuntu/assets/4024414/4a6aad97-534e-42c0-916e-f028f10c3bf0)

#
#
        
        sangbinlee9@master:/etc/nginx/sites-enabled$ ll
        total 8
        drwxr-xr-x 2 root root 4096 Nov 20 02:10 ./
        drwxr-xr-x 8 root root 4096 Nov 20 04:32 ../
        lrwxrwxrwx 1 root root   34 Nov 20 01:51 default -> /etc/nginx/sites-available/default
        lrwxrwxrwx 1 root root   38 Nov 20 02:10 sodi9.store -> /etc/nginx/sites-available/sodi9.store
        sangbinlee9@master:/etc/nginx/sites-enabled$ pwd
        /etc/nginx/sites-enabled
        sangbinlee9@master:/etc/nginx/sites-enabled$

#
# vi /etc/nginx/sites-available/sodi9.store
        sangbinlee9@master:~$ vi /etc/nginx/sites-available/sodi9.store

![image](https://github.com/sangbinlee/reactjs-nginx-ubuntu/assets/4024414/2c9c3cae-811a-4056-b123-0985283941d9)




![image](https://github.com/sangbinlee/reactjs-nginx-ubuntu/assets/4024414/3f77fd59-10bd-4ea6-8457-3409ac527af4)


![image](https://github.com/sangbinlee/reactjs-nginx-ubuntu/assets/4024414/34d4285c-eaf4-41bd-bf74-81579f88c8f6)

![image](https://github.com/sangbinlee/reactjs-nginx-ubuntu/assets/4024414/bbec151d-68ec-48d4-8e57-5c3b8cb1a716)


![image](https://github.com/sangbinlee/reactjs-nginx-ubuntu/assets/4024414/0cea6ffa-bec0-4fe7-9ce9-957720f69dbb)

#
#

#
#

#
#  sudo vi index.nginx-debian.html
        
        sangbinlee9@master:/var/www/html$ sudo vi index.nginx-debian.html
        [sudo] password for sangbinlee9:
        sangbinlee9@master:/var/www/html$


![image](https://github.com/sangbinlee/reactjs-nginx-ubuntu/assets/4024414/6b9607a4-a45b-4aff-ab0a-97b049a1a048)




![image](https://github.com/sangbinlee/reactjs-nginx-ubuntu/assets/4024414/34e9e70a-a0e1-446e-bcbf-b42fa46812e2)

#
#

#
#

#
#
