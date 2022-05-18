FROM nginx:1.21-alpine

ENV USER=app
ENV UID=1000
ENV GID=1000

RUN addgroup -g $GID $USER
RUN adduser \
    --disabled-password \
    --gecos "" \
    --home "$(pwd)" \
    --ingroup "$USER" \
    --no-create-home \
    --uid "$UID" \
    "$USER"


RUN touch /var/run/nginx.pid

COPY ./conf/nginx.conf /etc/nginx/
COPY ./conf/default.conf /etc/nginx/conf.d/

RUN mkdir -p /etc/nginx/html /var/www/html \
  && chown -R app:app /etc/nginx /var/www /var/cache/nginx /var/run/nginx.pid
EXPOSE 80
USER app:app
WORKDIR var/www/html