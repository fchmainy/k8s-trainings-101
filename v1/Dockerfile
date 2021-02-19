FROM nginx:mainline-alpine
RUN rm /etc/nginx/conf.d/*
ADD hello.conf /etc/nginx/conf.d/
ADD *.png /usr/share/nginx/html/
ADD index.php /usr/share/nginx/html/
