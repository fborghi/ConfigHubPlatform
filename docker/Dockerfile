FROM        tomee:8.0.16-plus

# Can set version arguments with --build-arg when calling 'docker build'
ARG         VERSION
ARG         DB_VERSION

COPY        ./rest/target/ROOT.war /webapps/confighub.war
COPY        ./docker/config/lib/* /lib
COPY        ./docker/config/cert /cert

RUN         if [ -z $VERSION ]; then VERSION=$(curl -s https://api.github.com/repos/ConfigHubPub/ConfigHubPlatform/releases/latest | grep tag_name | grep -oe '[\.0-9]*'); fi \
            && if [ -z $DB_VERSION ]; then DB_VERSION=$(curl -s https://api.github.com/repos/ConfigHubPub/Database-Manager/releases/latest | grep tag_name | grep -oe '[\.0-9]*'); fi \
            && wget -q https://github.com/ConfigHubPub/ConfigHubPlatform/releases/download/v${VERSION}/confighub-v${VERSION}.tar.gz \
            && tar -xzvf confighub-v${VERSION}.tar.gz \
            && rm confighub-v${VERSION}.tar.gz \
            && mv confighub-v${VERSION} confighub \
            && wget -O ConfigHubDBManager.jar -q https://github.com/ConfigHubPub/Database-Manager/releases/download/v${DB_VERSION}/ConfigHubDBManager-${DB_VERSION}.jar

EXPOSE      80 443
