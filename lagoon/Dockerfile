FROM uselagoon/lagoon-cli:latest as lagoon-cli

FROM uselagoon/php-8.2-cli-drupal:latest as cli

COPY --from=lagoon-cli /lagoon /usr/local/bin/lagoon

COPY composer.* /app/
COPY assets /app/assets

RUN composer install \
     --no-dev

COPY . /app

RUN mkdir -p -v -m775 /app/web/sites/default/files
RUN mkdir -p /tmp/drush

# Define where the Drupal Root is located
ENV WEBROOT=web

FROM uselagoon/nginx-drupal:latest as nginx

COPY --from=cli /app /app

ENV WEBROOT=web

FROM uselagoon/php-8.2-fpm:latest as php
 
COPY --from=cli /app /app
