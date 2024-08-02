FROM uselagoon/lagoon-cli:latest as lagoon-cli

FROM uselagoon/php-8.3-cli-drupal:latest as cli

COPY --from=lagoon-cli /lagoon /usr/local/bin/lagoon

COPY composer.* /app/
COPY assets /app/assets

RUN composer install \
     --no-dev

COPY . /app

RUN fix-permissions /app/web/sites/default/settings.php

RUN mkdir -p -v -m775 /app/web/sites/default/files

# Define where the Drupal Root is located
ENV WEBROOT=web

FROM uselagoon/nginx-drupal:latest as nginx

COPY --from=cli /app /app

ENV WEBROOT=web

FROM uselagoon/php-8.3-fpm:latest as php
 
COPY --from=cli /app /app
