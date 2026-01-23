FROM node:20-alpine

# add curl for healthcheck
RUN apk add --no-cache curl tini

WORKDIR /app

# have nodemon available for local dev use (file watching)
RUN npm install -g nodemon

COPY package*.json ./

RUN npm install --only=production

COPY . .

ENV PORT 4000

EXPOSE 4000

CMD ["/sbin/tini", "--", "node", "server.js"]
