FROM redis:6.0.5

ENV REDIS_PASSWORD=secret

CMD ["sh", "-c", "exec redis-server --requirepass \"$REDIS_PASSWORD\""]

WORKDIR /data