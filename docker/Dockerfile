FROM        tomee:8.0.16-plus

# Can set version arguments with --build-arg when calling 'docker build'
ARG         VERSION
ARG         DB_VERSION

COPY        ./rest/target/ROOT.war /usr/local/tomee/webapps/ROOT.war

RUN         apt-get update \
            && apt-get -y install gettext-base \
            && if [ -z $VERSION ]; then VERSION=$(curl -s https://api.github.com/repos/ConfigHubPub/ConfigHubPlatform/releases/latest | grep tag_name | grep -oe '[\.0-9]*'); fi \
            && if [ -z $DB_VERSION ]; then DB_VERSION=$(curl -s https://api.github.com/repos/ConfigHubPub/Database-Manager/releases/latest | grep tag_name | grep -oe '[\.0-9]*'); fi \
            && wget -O ConfigHubDBManager.jar -q https://github.com/ConfigHubPub/Database-Manager/releases/download/v${DB_VERSION}/ConfigHubDBManager-${DB_VERSION}.jar

COPY        ./docker/layer/init.sh /usr/local/tomee/init.sh
COPY        ./docker/config/cert /usr/local/tomee/cert
COPY        ./docker/config/lib/* /usr/local/tomee/lib/
COPY        ./docker/config/conf/* /usr/local/tomee/conf/

EXPOSE      80 443

ENTRYPOINT [ "/usr/local/tomee/init.sh" ]