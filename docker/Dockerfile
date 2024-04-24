FROM        openjdk:8-alpine

# Can set version arguments with --build-arg when calling 'docker build'
ARG         VERSION
ARG         DB_VERSION

COPY        ./rest/target/ROOT.zip .

RUN         apt update && apt install --no-cache \
                bash \
                curl \
                gettext \
                py-pip \
                python \
                wget \
                zip \
                unzip \
            && pip install supervisor \
            && mkdir /var/log/supervisord \
            # Fetch latest tagged versions if not specified
            && unzip ROOT.zip ROOT \
            && ls \
            && rm ROOT.zip \
            && mv ROOT confighub\server \
            && wget -O ConfigHubDBManager.jar -q https://github.com/ConfigHubPub/Database-Manager/releases/download/v1.9.2/ConfigHubDBManager-1.9.2.jar
            # Fixes a bug where /bin/sh on alpine can't do <<<.

COPY        ./docker/layer /

EXPOSE      80 443
ENTRYPOINT ["/init.sh"]
