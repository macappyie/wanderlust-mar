#stage1:
FROM node:24 AS backend-builder

#setup up working directory
WORKDIR /app

#copy code
COPY . .

RUN npm i

#test
RUN npm run test

#stage2:
FROM node:24-slim
#setup working directory
WORKDIR /app
#copy the above stage as compressed
COPY --from=backend-builder /app .
COPY .env.docker .env
#expose
EXPOSE 5000
#app
CMD ["npm", "start"]

