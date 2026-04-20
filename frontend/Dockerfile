FROM node:20-alpine
WORKDIR /app
COPY . .
RUN  npm install && rm -rf /var/cache/*
EXPOSE 8080
CMD ["npm", "run", "serve"]