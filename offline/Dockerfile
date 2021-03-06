FROM nginx:1.19-alpine
ENV LANG=C.UTF-8

COPY default.conf /etc/nginx/conf.d/default.conf
COPY kubernetes-yum /kubernetes/yum
COPY kubernetes-1.20.1.tar /kubernetes/kubernetes-1.20.1.tar

RUN apk add --no-cache \
        git \
        bash \
        rsync \
        openssl \
        sshpass \
        py3-pip \
        openssh-client \
        ca-certificates; \
    apk add --no-cache --virtual .build-deps \
        build-base \
        libffi-dev \
        openssl-dev \
        python3-dev; \
    pip3 install --no-cache-dir --upgrade pip cffi; \
    pip3 install --no-cache-dir --upgrade pycrypto pywinrm; \
    pip3 install --no-cache-dir ansible==2.10.4 netaddr; \
    apk del .build-deps; \
    git clone https://github.com/TimeBye/kubeadm-ha.git /etc/ansible; \
    curl -sSLo /kubernetes/helm-v3.4.2-linux-amd64.tar.gz https://get.helm.sh/helm-v3.4.2-linux-amd64.tar.gz

# Add Aliyun mirror source
RUN cp /etc/apk/repositories /etc/apk/repositories.bak; \
    sed -i 's dl-cdn.alpinelinux.org mirrors.aliyun.com g' /etc/apk/repositories

WORKDIR /etc/ansible