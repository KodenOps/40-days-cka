# This is a javascript application, so we will be going with node
FROM node:18-alpine

WORKDIR /app

COPY . .
# install all the application dependencies
RUN apk add --no-cache python3 py3-pip make g++ sqlite-dev


RUN yarn install --production

CMD ["node", "src/index.js"]

EXPOSE 3001