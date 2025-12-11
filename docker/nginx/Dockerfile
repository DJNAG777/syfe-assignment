# --- Stage 1: Builder ---
FROM ubuntu:20.04 AS builder

# Avoid prompts during installation
ENV DEBIAN_FRONTEND=noninteractive

# 1. Install build tools and dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpcre3-dev \
    libssl-dev \
    perl \
    make \
    wget \
    zlib1g-dev \
    libncurses5-dev \
    libreadline-dev \
    git \
    libpq-dev

# 2. Download OpenResty (contains Nginx + Lua)
WORKDIR /tmp
ARG RESTY_VERSION="1.21.4.1"
RUN wget https://openresty.org/download/openresty-${RESTY_VERSION}.tar.gz && \
    tar -xzvf openresty-${RESTY_VERSION}.tar.gz

WORKDIR /tmp/openresty-${RESTY_VERSION}

# 3. Compile with the EXACT flags from the assignment
RUN ./configure --prefix=/opt/openresty \
    --with-pcre-jit \
    --with-ipv6 \
    --without-http_redis2_module \
    --with-http_iconv_module \
    --with-http_postgres_module \
    --with-http_stub_status_module \
    -j8 && \
    make && \
    make install

# --- Stage 2: Final Image ---
FROM ubuntu:20.04

# Install runtime libraries
RUN apt-get update && apt-get install -y \
    libssl-dev libpcre3 zlib1g ca-certificates libpq5 && \
    rm -rf /var/lib/apt/lists/*

# Copy the compiled application
COPY --from=builder /opt/openresty /opt/openresty

# Create symlinks so we can just type 'nginx'
RUN ln -sf /opt/openresty/nginx/sbin/nginx /usr/local/bin/nginx

# Forward logs to Docker (Standard Practice)
RUN ln -sf /dev/stdout /opt/openresty/nginx/logs/access.log && \
    ln -sf /dev/stderr /opt/openresty/nginx/logs/error.log

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
