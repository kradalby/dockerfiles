FROM nginx:alpine as builder

RUN apk add --no-cache --virtual .build-deps \
      curl \
      gcc \
      gd-dev \
      geoip-dev \
      gnupg \
      libc-dev \
      libxslt-dev \
      linux-headers \
      make \
      openldap-dev \
      pcre-dev \
      tar \
      unzip \
      zlib-dev

ENV NGINX_NDK_VERSION 0.3.1

RUN mkdir -p /tmp/src \
        && curl -fSL https://nginx.org/download/nginx-$NGINX_VERSION.tar.gz -o nginx.tar.gz \
        && tar -zxC /tmp/src -f nginx.tar.gz \
        && curl -fsSL https://github.com/kvspb/nginx-auth-ldap/archive/master.zip -o /tmp/nginx-auth-ldap-master.zip \
        && unzip -d /tmp/src /tmp/nginx-auth-ldap-master.zip \
        && curl -fsSL https://github.com/simpl/ngx_devel_kit/archive/v${NGINX_NDK_VERSION}.zip -o /tmp/nginx-ndk-${NGINX_NDK_VERSION}.zip \
        && unzip -d /tmp/src /tmp/nginx-ndk-${NGINX_NDK_VERSION}.zip

WORKDIR /tmp/src/nginx-${NGINX_VERSION}

RUN ./configure \
        --with-http_ssl_module \
        --with-compat \
        --add-dynamic-module=/tmp/src/nginx-auth-ldap-master \
        --add-dynamic-module=/tmp/src/ngx_devel_kit-${NGINX_NDK_VERSION} \
        && make modules


FROM nginx:alpine as production

RUN apk add --no-cache nginx-mod-http-lua

COPY --from=builder /tmp/src/nginx-$NGINX_VERSION/objs/ngx_http_auth_ldap_module.so /usr/lib/nginx/modules/
COPY --from=builder /tmp/src/nginx-$NGINX_VERSION/objs/ndk_http_module.so /usr/lib/nginx/modules/

ADD https://github.com/knyar/nginx-lua-prometheus/raw/master/prometheus.lua /etc/nginx/lua/prometheus.lua

COPY ./metrics.conf /etc/nginx/conf.d/

RUN apk add --no-cache libldap \
    && sed -i '1i lua_load_resty_core off;' /etc/nginx/nginx.conf \
    && sed -i '1i include /etc/nginx/ldap.conf;' /etc/nginx/nginx.conf \
    && sed -i '1i load_module "/usr/lib/nginx/modules/ngx_http_auth_ldap_module.so";' /etc/nginx/nginx.conf \
    && sed -i '1i load_module "/usr/lib/nginx/modules/ngx_http_lua_module.so";' /etc/nginx/nginx.conf \
    && sed -i '1i load_module "/usr/lib/nginx/modules/ndk_http_module.so";' /etc/nginx/nginx.conf \
    && touch /etc/nginx/ldap.conf \
    && rm -fr \
        /etc/nginx/*.default \
        /tmp/* \
        /var/tmp/* \
        /var/cache/apk/*
