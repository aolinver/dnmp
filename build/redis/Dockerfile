FROM redis:alpine

# change source and timezone
RUN apk add -U --no-cache tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && apk del tzdata

CMD ["redis-server","/etc/redis/redis.conf"]