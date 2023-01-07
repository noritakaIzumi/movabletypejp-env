FROM perl:5.36.0-threaded-bullseye AS build

RUN apt update \
    && apt install -y libsasl2-dev libgd-dev libperl-dev

WORKDIR /root

# Devel::CheckLib
RUN echo "requires 'Devel::CheckLib';" > cpanfile \
    && cpanm --notest --installdeps .

# Image::Magick
RUN echo "requires 'Image::Magick', '< 7';" > cpanfile \
    && cp -va /usr/include/x86_64-linux-gnu/ImageMagick-6/magick/magick-baseconfig.h /usr/include/ImageMagick-6/magick/magick-baseconfig.h \
    && cpanm --notest --installdeps .

# Other modules
COPY cpanfile cpanfile
RUN cpanm --notest --installdeps .

FROM perl:5.36.0-slim-threaded-bullseye AS deploy

RUN apt update \
    && apt install -y fcgiwrap spawn-fcgi

RUN apt install -y libpq-dev libmariadb-dev libgd-dev libmagickcore-dev

COPY --from=build /usr/local/lib/perl5 /usr/local/lib/perl5

ARG FCGI_PORT=9000
RUN sed -ie "s/^FCGI_SOCKET=.*$/FCGI_PORT=\"${FCGI_PORT}\"\nFCGI_ADDR=\"0.0.0.0\"/g" /etc/init.d/fcgiwrap
# 無くても動いたが、意味的にポートを解放しておくべき
EXPOSE ${FCGI_PORT}

CMD ["bash", "-c", "service fcgiwrap start && tail -f /dev/null"]
