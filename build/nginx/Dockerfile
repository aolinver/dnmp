FROM nginx:stable-alpine

# change timezone
RUN apk add -U --no-cache tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && apk del tzdata