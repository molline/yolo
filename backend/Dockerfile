FROM  node:16.17.0-bullseye-slim

WORKDIR /backend

ADD package.json package.json

RUN npm install

COPY . /backend

EXPOSE 5000

CMD ["npm","run" , "start"]



