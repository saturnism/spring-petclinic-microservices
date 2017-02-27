FROM java:8
VOLUME /tmp
ADD https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh wait-for-it.sh
RUN bash -c 'chmod +x wait-for-it.sh'

ARG ARTIFACT_VERSION
RUN mkdir /opt/cdbg && \
    wget -qO- https://storage.googleapis.com/cloud-debugger/compute-java/debian-wheezy/cdbg_java_agent_service_account.tar.gz | \
    tar xvz -C /opt/cdbg

ARG ARTIFACT_NAME
ENV ARTIFACT_VERSION ${ARTIFACT_VERSION}
ENV ARTIFACT_NAME ${ARTIFACT_NAME}

ADD ${ARTIFACT_NAME}.jar /app.jar
ENV SPRING_PROFILES_ACTIVE docker
RUN bash -c 'touch /app.jar'
EXPOSE 8081
ENTRYPOINT ["/bin/sh", "-c", "java -agentpath:/opt/cdbg/cdbg_java_agent.so==--logtostderr=1 -Dcom.google.cdbg.module=${ARTIFACT_NAME} -Dcom.google.cdbg.version=${ARTIFACT_VERSION} -Djava.security.egd=file:/dev/./urandom -jar /app.jar"]
