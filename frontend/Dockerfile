FROM node:24.11.1-slim AS development-dependencies-env
WORKDIR /app
COPY package*.json /app
COPY ./run.sh /app
RUN chmod +x ./run.sh
CMD ["sh","./run.sh"]

FROM node:24.11.1-slim AS production-dependencies-env
COPY ./package.json package-lock.json /app/
WORKDIR /app
RUN npm ci --omit=dev

FROM node:24.11.1-slim AS build-env
COPY . /app/
COPY --from=development-dependencies-env /app/node_modules /app/node_modules
WORKDIR /app
RUN npm run build

FROM ghcr.io/static-web-server/static-web-server:2 as production

COPY --from=build-env /app/dist /public/ui
