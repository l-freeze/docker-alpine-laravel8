# clone 
```
git clone git@github.com:l-freeze/docker-alpine-laravel8.git l-freeze
```

# .env
任意で書き換え可能だが、操作内容は書き換えない事を前提としている
```
APP_NAME=lfreeze
MYSQL_DATABASE=lfreeze
MYSQL_USER=lfreeze
MYSQL_PASSWORD=lfreeze
MYSQL_ROOT_PASSWORD=lfreeze
```

# docker-compose.yml
- networks は任意に変更

> networksのサブネット`192.168.20`の部分を書き換えたら、gatewayと各コンテナのipv4_addressも同じように書き換える事

# build
```
docker-compose up -d --build
```

# laravel install
```sh
docker exec -it lfreeze-php sh appinit.sh

----------------------------------------------
    |     |         |
    |,---.|--- ,---.|--- ,---.,---.,---.,-.-.
    ||---'|    `---.|    |    |---',---|| | |
`---'`---'`---'`---'`---'`    `---'`---^` ' '


Which Jetstream stack do you prefer?
  [0] livewire
  [1] inertia
 > 0

 Will your application use teams? (yes/no) [no]:
 > no
----------------------------------------------

docker exec -it lfreeze-node sh -c "npm install && npm run dev"
```


# DBに繋がらない時

```docker
docker-compose down --volumes
docker-compose up -d --build
```


