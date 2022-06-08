FROM ubuntu:22.04
ARG KUBECTL=1.23.0
ENV CREDENTIALS=admin:admin
# install curl
RUN apt update && apt install curl -y
RUN curl -LOs https://github.com/tsl0922/ttyd/releases/download/1.6.3/ttyd.x86_64 && \
    mv ttyd.x86_64 /usr/local/bin/ttyd && \
    chmod +x /usr/local/bin/ttyd
RUN curl -LOs https://storage.googleapis.com/kubernetes-release/release/v${KUBECTL}/bin/linux/amd64/kubectl && \
    chmod +x kubectl && \
    mv ./kubectl /usr/local/bin/kubectl
RUN ttyd --version

ENTRYPOINT ["sh","-c", "ttyd -p 8080 --credential $CREDENTIALS bash"]
