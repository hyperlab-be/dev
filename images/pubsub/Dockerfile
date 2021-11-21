FROM google/cloud-sdk:alpine

RUN apk --update add openjdk8-jre \
    && gcloud components install pubsub-emulator beta --quiet

COPY start-container /usr/local/bin/start-container

RUN chmod +x /usr/local/bin/start-container

EXPOSE 8085

VOLUME /opt/data

ENTRYPOINT ["start-container"]