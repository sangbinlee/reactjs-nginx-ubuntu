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
