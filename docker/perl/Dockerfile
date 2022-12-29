FROM perl:5.36.0-slim-threaded-bullseye

RUN apt update \
    && apt install -y fcgiwrap spawn-fcgi

ENV FCGI_PORT=9000
RUN sed -ie "s/^FCGI_SOCKET=.*$/FCGI_PORT=\"${FCGI_PORT}\"\nFCGI_ADDR=\"0.0.0.0\"/g" /etc/init.d/fcgiwrap
# 無くても動いたが、意味的にポートを解放しておくべき
EXPOSE ${FCGI_PORT}

CMD ["bash", "-c", "service fcgiwrap start && tail -f /dev/null"]
