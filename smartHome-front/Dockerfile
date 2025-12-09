# ---------- Stage 1 : build Angular ----------
FROM node:14.15.0-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

# ---------- Stage 2 : servir avec Nginx ----------
FROM nginx:alpine

# ⚠️ adapte le chemin si ton dossier dist a un autre nom
COPY --from=builder /app/dist/smart-home /usr/share/nginx/html

EXPOSE 80
