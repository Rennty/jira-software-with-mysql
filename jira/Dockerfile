From debian:9
MAINTAINER Sovanna Hing <sovanna.hing@gmail.com>

RUN apt-get update \
  && apt-get install -y locales \
  && rm -rf /var/lib/apt/lists/* \
  && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8

ENV LANG en_US.utf8
ENV JIRA_HOME /var/lib/atlassian/jira
ENV JIRA_INSTALL /opt/atlassian/jira

RUN apt-get update \
  && apt-get install -y curl software-properties-common dirmngr debconf-utils \
  && add-apt-repository "deb http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main" \
  && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys C2518248EEA14886 \
  && apt-get update \
  && echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections \
  && echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections \
  && apt-get install -y oracle-java8-set-default \
  && useradd --create-home --comment "Account for running JIRA Software" -s /bin/bash jira \
  && mkdir -p "${JIRA_INSTALL}" \
  && curl -L https://www.atlassian.com/software/jira/downloads/binary/atlassian-jira-software-7.9.0.tar.gz | tar -xzv --directory "${JIRA_INSTALL}" --strip-components=1 \
  && curl -L https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.46.tar.gz | tar -xzv --directory "${JIRA_INSTALL}/lib" --strip-components=1 "mysql-connector-java-5.1.46/mysql-connector-java-5.1.46-bin.jar" \
  && chown -R jira "${JIRA_INSTALL}" \
  && chmod -R u=rwx,go-rwx "${JIRA_INSTALL}" \
  && mkdir -p "${JIRA_HOME}" \
  && chown -R jira "${JIRA_HOME}" \
  && chmod -R u=rwx,go-rwx "${JIRA_HOME}"

ENV JAVA_HOME /usr/lib/jvm/java-8-oracle
RUN export JAVA_HOME

USER jira

CMD ["/opt/atlassian/jira/bin/start-jira.sh", "-fg"]
