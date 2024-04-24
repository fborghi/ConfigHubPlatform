FROM        openjdk:8-alpine

# Can set version arguments with --build-arg when calling 'docker build'
ARG         VERSION
ARG         DB_VERSION

COPY        ./rest/target/ROOT.zip .

RUN         apk update && apk add --no-cache \
                bash \
                curl \
                gettext \
                py-pip \
                python \
                wget \
            && pip install supervisor \
            && mkdir /var/log/supervisord \
            # Fetch latest tagged versions if not specified
            && if [ -z $VERSION ]; then VERSION=$(curl -s https://api.github.com/repos/ConfigHubPub/ConfigHubPlatform/releases/latest | grep tag_name | grep -oe '[\.0-9]*'); fi \
            && if [ -z $DB_VERSION ]; then DB_VERSION=$(curl -s https://api.github.com/repos/ConfigHubPub/Database-Manager/releases/latest | grep tag_name | grep -oe '[\.0-9]*'); fi \
            && unzip ROOT.zip \
            && rm ROOT.zip \
            && mv ROOT confighub\server \
            && wget -O ConfigHubDBManager.jar -q https://github.com/ConfigHubPub/Database-Manager/releases/download/v${DB_VERSION}/ConfigHubDBManager-${DB_VERSION}.jar
            # Fixes a bug where /bin/sh on alpine can't do <<<.

COPY        ./docker/layer /

EXPOSE      80 443
ENTRYPOINT ["/init.sh"]
