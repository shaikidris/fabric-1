/*
 * Copyright 2016 ANI Technologies Pvt. Ltd.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

FROM ubuntu:14.04
MAINTAINER guruprasad.sridharan@olacabs.com

RUN apt-get update \
  && apt-get install -y --no-install-recommends software-properties-common \
  && add-apt-repository ppa:webupd8team/java \
  && gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 \
  && apt-get update \
  && echo debconf shared/accepted-oracle-license-v1-1 select true |  debconf-set-selections \
  && echo debconf shared/accepted-oracle-license-v1-1 seen true |  debconf-set-selections \
  && apt-get install -y --no-install-recommends oracle-java8-installer \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

EXPOSE 8080

ENV basedir /executor

COPY fabric-executor-*.jar $basedir/fabric-executor.jar

WORKDIR $basedir

RUN mkdir /var/log/fabric-executor

RUN touch /var/log/fabric-executor/stdout.log

RUN touch /var/log/fabric-executor/stderr.log

RUN ln -sf /dev/stdout /var/log/fabric-executor/stdout.log

RUN ln -sf /dev/stderr /var/log/fabric-executor/stderr.log

CMD ["sh", "-c", "java -Xmx${HEAP_SIZE} -Xms${HEAP_SIZE} -Dfile.encoding=utf-8 ${JVM_OPTS} -jar fabric-executor.jar -d ${METRICS_DISABLED} --spec ${SPEC_LOCATION} -m ${OPENTSDB_URL}"]

