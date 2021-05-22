FROM node:15-alpine
ARG APP_NAME
ARG APP_ROOT

RUN mkdir /var/command
WORKDIR /var/command
RUN echo -e " \
cd $APP_ROOT \n\
npm update \n\
npm install \n\
npm install -g laravel-echo-server \n\
npm install --save-dev socket.io-client@2 \n\
laravel-echo-server init \n\
echo --end-- \n\
printf '\e[32;1m[%s] \e[36m%s\e\\\n[m' '(node container) Command' 'vi laravel-echo-server.json' \n\
printf '\e[32;1m[%s] \e[36m%s\e\\\n[m' 'modify' 'databaseConfig: {redis:{\"host\":\"redis\"},}'  \n\
printf '\e[32m[%s] \e[37m%s\e\\\n[m' '(node container) Command' 'Ctrl + d' \n\
" > appinit.sh


WORKDIR ${APP_ROOT}/${APP_ROOT}