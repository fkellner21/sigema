# -----------------------------------
# BUILD STAGE
# -----------------------------------
FROM node:20 AS build
WORKDIR /app

COPY package.json package-lock.json ./
RUN npm install

COPY . .
RUN npm run build --prod

# -----------------------------------
# RUNTIME STAGE - NGINX
# -----------------------------------
FROM nginx:stable 

# Eliminar config por defecto
RUN rm -rf /etc/nginx/conf.d/default.conf

# Copiar nuestra config para Angular
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copiar la app Angular compilada
COPY --from=build /app/dist/maquinas/browser/ /usr/share/nginx/html
COPY --from=build /app/dist/maquinas/*.txt /usr/share/nginx/html
COPY --from=build /app/dist/maquinas/*.json /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
