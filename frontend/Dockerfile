#stage1: build stage
FROM node:24 AS frontend-builder

#set working directory to /app
WORKDIR /app

#copy package*.json for dependency installation
COPY package*.json ./

#install dependencies
RUN npm install


#copy rest of app code
COPY . .

#stage2: final stage
FROM node:24-slim


#set working directory /app
WORKDIR /app

#copy build assets and dependencies from frontend-builder stage
COPY --from=frontend-builder /app .

#copy .env.sample file to .env.local
COPY .env.sample .env.local

#expose port 5173
EXPOSE 5173

#define default command to run app in development
CMD ["npm", "run", "dev", "--", "--host"]

