FROM node:15

WORKDIR /usr/src/app

RUN apt-get update && apt-get install -y wget

#Espera uma determinada ação como o banco estar pronto para que a imagem possa subir
ENV DOCKERIZE_VERSION v0.6.1
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
  && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
  && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz


EXPOSE 3000

CMD ["node","index.js"]