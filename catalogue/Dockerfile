FROM node:14

EXPOSE 7081

WORKDIR /opt/server

COPY package.json /opt/server/

# npm install get the dependencies of node js project
RUN npm install 

COPY server.js /opt/server/

ENTRYPOINT ["node", "server.js"]

