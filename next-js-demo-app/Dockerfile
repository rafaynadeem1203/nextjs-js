FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json .
RUN npm ci
COPY . .
RUN npm run build

FROM node:20-alpine AS production

WORKDIR /app

RUN addgroup -g 1001 -S nodejs && \
    adduser -S nextjs -u 1001 && \
    chown -R nextjs:nodejs /app

USER nextjs

COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/package*.json ./

COPY --from=builder /app/.next/static ./.next/static
COPY --from=builder /app/public /app/public

EXPOSE 3000

CMD ["node", "server.js"]

