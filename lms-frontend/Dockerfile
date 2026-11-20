# ---------- ?tape 1 : build Angular ----------
FROM node:16-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm install --legacy-peer-deps
COPY . .
RUN npm run build

# ---------- ?tape 2 : Nginx ----------
FROM nginx:alpine
COPY --from=build /app/dist/ /tmp/dist/
RUN cp -r /tmp/dist/*/* /usr/share/nginx/html/ && rm -rf /tmp/dist
EXPOSE 80
