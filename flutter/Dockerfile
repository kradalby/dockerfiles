FROM cirrusci/flutter:base

# root user is needed by drone
USER root

ENV FLUTTER_HOME ${HOME}/sdks/flutter
ENV FLUTTER_VERSION 1.0.0

RUN git clone --branch v${FLUTTER_VERSION} https://github.com/flutter/flutter.git ${FLUTTER_HOME}

ENV PATH ${PATH}:${FLUTTER_HOME}/bin:${FLUTTER_HOME}/bin/cache/dart-sdk/bin

RUN yes | sdkmanager \
    "platforms;android-28" \
    "build-tools;28.0.3"
RUN yes | sdkmanager \
    "platforms;android-27" \
    "build-tools;28.0.3"

# doctor
RUN flutter doctor
