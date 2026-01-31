# Dockerfile.nginx-gateway
FROM nginx:alpine

# Удаляем дефолтный конфиг и скрипт который его меняет
RUN rm -f /etc/nginx/conf.d/default.conf && \
    rm -f /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh && \
    rm -f /docker-entrypoint.d/20-envsubst-on-templates.sh

# Устанавливаем curl для healthcheck и дополнительные утилиты
RUN apk add --no-cache curl jq

# Копируем конфигурации
COPY ./nginx/nginx.conf /etc/nginx/nginx.conf
COPY ./nginx/conf.d/ /etc/nginx/conf.d/
#COPY ssl/ /etc/nginx/ssl/

# Создаем папки для логов
RUN mkdir -p /var/log/nginx && \
    mkdir -p /var/cache/nginx && \
    mkdir -p /var/run && \
    chown -R nginx:nginx /var/log/nginx && \
    chown -R nginx:nginx /var/cache/nginx && \
    chown -R nginx:nginx /var/run && \
    chmod -R 755 /var/cache/nginx && \
    chmod -R 755 /var/log/nginx && \
    chmod -R 755 /var/run && \
    touch /var/run/nginx.pid && \
    chown nginx:nginx /var/run/nginx.pid


# Healthcheck
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:80/health || exit 1

# Пользователь от имени которого работает
USER nginx

# Порт
EXPOSE 80 443

# Запуск
CMD ["nginx", "-g", "daemon off;"]