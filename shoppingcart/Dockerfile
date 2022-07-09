FROM node:14-alpine
 WORKDIR /usr/src/app
 COPY package*.json ./
 RUN npm install
 RUN npm audit fix 
 RUN apk update && apk add vim
 COPY . .
 EXPOSE 5000
 CMD ["node", "app.js"]

