
FROM amazonlinux:2

ARG version=1.8.0_412.b08-1
ARG DB_VERSION

RUN set -eux \
    && export GNUPGHOME="$(mktemp -d)" \
    && curl -fL -o corretto.key https://yum.corretto.aws/corretto.key \
    && gpg --batch --import corretto.key \
    && gpg --batch --export --armor '6DC3636DAE534049C8B94623A122542AB04F24E3' > corretto.key \
    && rpm --import corretto.key \
    && rm -r "$GNUPGHOME" corretto.key \
    && curl -fL -o /etc/yum.repos.d/corretto.repo https://yum.corretto.aws/corretto.repo \
    && grep -q '^gpgcheck=1' /etc/yum.repos.d/corretto.repo \
    && echo "priority=9" >> /etc/yum.repos.d/corretto.repo \
    && yum install -y java-1.8.0-amazon-corretto-devel-$version \
    && (find /usr/lib/jvm/java-1.8.0-amazon-corretto -name src.zip -delete || true) \
    && yum install -y fontconfig \
    && yum install -y gettext \
    && yum install -y wget \
    && yum clean all

ENV         LANG C.UTF-8
ENV         JAVA_HOME=/usr/lib/jvm/java-1.8.0-amazon-corretto

WORKDIR     /usr/local/src
COPY        ./rest/target/apache-tomee confighub/

WORKDIR     /usr/local/src/confighub

RUN         if [ -z $DB_VERSION ]; then DB_VERSION=$(curl -s https://api.github.com/repos/ConfigHubPub/Database-Manager/releases/latest | grep tag_name | grep -oe '[\.0-9]*'); fi \
            && wget -O ConfigHubDBManager.jar -q https://github.com/ConfigHubPub/Database-Manager/releases/download/v${DB_VERSION}/ConfigHubDBManager-${DB_VERSION}.jar

COPY        ./docker/layer/init.sh init.sh
COPY        ./docker/config/cert cert/

EXPOSE      80 443

ENTRYPOINT [ "./init.sh" ]