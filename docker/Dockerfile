
FROM amazonlinux:2

ARG version=1.8.0_412.b08-1
# In addition to installing the Amazon corretto, we also install
# fontconfig. The folks who manage the docker hub's
# official image library have found that font management
# is a common usecase, and painpoint, and have
# recommended that Java images include font support.
#
# See:
#  https://github.com/docker-library/official-images/blob/master/test/tests/java-uimanager-font/container.java

# The logic and code related to Fingerprint is contributed by @tianon in a Github PR's Conversation
# Comment = https://github.com/docker-library/official-images/pull/7459#issuecomment-592242757
# PR = https://github.com/docker-library/official-images/pull/7459
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
    && yum install -y gettext-base \
    && yum clean all \
    && mkdir server

ENV LANG C.UTF-8
ENV JAVA_HOME=/usr/lib/jvm/java-1.8.0-amazon-corretto


COPY        ./rest/target/apache-tomee/* /server

RUN         if [ -z $VERSION ]; then VERSION=$(curl -s https://api.github.com/repos/ConfigHubPub/ConfigHubPlatform/releases/latest | grep tag_name | grep -oe '[\.0-9]*'); fi \
            && if [ -z $DB_VERSION ]; then DB_VERSION=$(curl -s https://api.github.com/repos/ConfigHubPub/Database-Manager/releases/latest | grep tag_name | grep -oe '[\.0-9]*'); fi \
            && wget -O ConfigHubDBManager.jar -q https://github.com/ConfigHubPub/Database-Manager/releases/download/v${DB_VERSION}/ConfigHubDBManager-${DB_VERSION}.jar

COPY        ./docker/layer/init.sh /usr/local/tomee/init.sh
COPY        ./docker/config/cert /usr/local/tomee/cert

EXPOSE      80 443

ENTRYPOINT [ "/usr/local/tomee/init.sh" ]