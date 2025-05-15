FROM node:20-alpine AS base
WORKDIR /app
COPY package.json pnpm-lock.yaml ./
RUN npm install -g pnpm
RUN pnpm install

FROM base AS development
COPY . .
CMD ["pnpm", "run", "start:dev", "student-serivice"]

FROM base AS build
COPY . .
RUN pnpm run build student-serivice

FROM node:20-alpine AS production
WORKDIR /app
COPY --from=build /app/dist ./dist
COPY --from=build /app/node_modules ./node_modules
COPY --from=build /app/package.json ./

ARG SERVICE_NAME
ENV NODE_ENV=production

CMD ["node", "dist/apps/${SERVICE_NAME}/main"]

