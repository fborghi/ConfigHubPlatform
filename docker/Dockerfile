FROM        amazoncorretto:11-alpine3.18-jdk

# Can set version arguments with --build-arg when calling 'docker build'
ARG         VERSION
ARG         DB_VERSION

COPY        ./rest/target/ROOT.zip .

RUN         apk update && apk add --no-cache \
                bash \
                curl \
                gettext \
                py-pip \
                python3 \
                wget \
                zip \
                unzip \
            && pip install supervisor \
            && mkdir /var/log/supervisord \
            && echo 'File List' \
            && ls -la \
            # Fetch latest tagged versions if not specified
            && mkdir confighub \
            && mkdir confighub/server \
            && unzip ROOT.zip -d confighub/server \
            && rm ROOT.zip \
            && wget -O ConfigHubDBManager.jar -q https://github.com/ConfigHubPub/Database-Manager/releases/download/v1.9.2/ConfigHubDBManager-1.9.2.jar \
            # && rm /bin/sh && ln -s /bin/bash /bin/sh \
            && chmod -R +x /confighub/server
            # Fixes a bug where /bin/sh on alpine can't do <<<.

COPY        ./docker/layer /

EXPOSE      80 443
ENTRYPOINT ["/init.sh"]
