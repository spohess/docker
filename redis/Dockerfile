FROM redis:4.0

ENV REDIS_PASSWORD=secret

CMD ["sh", "-c", "exec redis-server --requirepass \"$REDIS_PASSWORD\""]

WORKDIR /var/www