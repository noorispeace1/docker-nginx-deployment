# Node 22 base image (some dependencies require Node >= 22)
FROM node:22-alpine

# All app files live in /app inside the container
WORKDIR /app

# openssl is needed by Prisma on Alpine; corepack gives us pnpm
RUN apk add --no-cache openssl \
    && corepack enable \
    && corepack prepare pnpm@10.20.0 --activate

# Install dependencies first (better layer caching)
COPY package.json pnpm-lock.yaml ./
RUN pnpm install --frozen-lockfile

# Copy the rest of the source code
COPY . .

# Generate Prisma client
RUN pnpm generate

# Express server port
EXPOSE 5000

# Run the app (tsx src/server.ts)
CMD ["pnpm", "start"]