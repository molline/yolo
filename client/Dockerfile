FROM  node:16.17.0-bullseye-slim

WORKDIR /client

COPY package.json package.json

RUN npm install

COPY . .

EXPOSE 3000

CMD npm start
# FROM node:16.17.0-bullseye-slim AS build-env
# COPY . /client
# WORKDIR /client

# COPY package.json package.json

# RUN npm install

# FROM gcr.io/distroless/nodejs16-debian11
# COPY --from=build-env /client /client
# WORKDIR /client
# CMD ["App.js"]

