FROM node:24-alpine AS builder
WORKDIR /app
COPY package*.json .
RUN npm ci --omit=dev
COPY . .
RUN npm run build

FROM builder AS production

WORKDIR /app

# RUN addgroup -g 1001 -S nodejs && \
#     adduser -S nextjs -u 1001 && \
#     chown -R nextjs:nodejs /app

# USER nextjs

COPY --from=builder /app/.next ./.next
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package*.json ./package*.json
COPY --from=builder /app/public ./public

EXPOSE 3000

CMD ["npm", "start"]

