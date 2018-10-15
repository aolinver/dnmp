FROM nginx:stable-alpine

# change source
RUN cp /etc/apk/repositories /etc/apk/repositories.bak \
	&& echo "http://mirrors.aliyun.com/alpine/v3.7/main/" > /etc/apk/repositories \
	&& echo "http://mirrors.aliyun.com/alpine/v3.7/community/" >> /etc/apk/repositories \
	&& apk add -U --no-cache tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && apk del tzdata