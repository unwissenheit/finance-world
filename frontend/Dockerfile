FROM node:20

WORKDIR /app

COPY package.json /app/package.json

COPY package-lock.json /app/package-lock.json

RUN npm i

CMD ["npx", "next", "dev", "-H", "0.0.0.0", "-p", "3000"]