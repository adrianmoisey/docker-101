FROM node:latest
ADD package.json .
RUN npm install
COPY server.js .
EXPOSE 8080
CMD ["npm", "start"]
