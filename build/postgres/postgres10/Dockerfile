FROM postgres:10-alpine

# 更改镜像源
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

# 更改为国内时区
RUN apk add --no-cache tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/shanghai" > /etc/timezone \
    && apk del tzdata \

    # 清除缓存
    && rm -rf /tmp/pear/download/* && rm -rf /tmp/pear/cache/* && rm -rf /var/cache/apk/* && rm -rf /var/lib/apk/*