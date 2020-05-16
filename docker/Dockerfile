# Copyright (C) 2016-2017 Bitergia
# GPLv3 License

FROM debian:stretch-slim
ENV DEBIAN_FRONTEND noninteractive

# install dependencies
RUN apt-get update && \
    apt-get -y install --no-install-recommends \
    bash locales \
    gcc \
    git git-core \
    python3 \
    python3-pip \
    python3-venv \
    python3-dev \
    python3-gdbm \
    mariadb-client \
    unzip curl wget sudo ssh \
    && \
    apt-get clean && \
    find /var/lib/apt/lists -type f -delete

RUN pip3 install --upgrade pip && \
    pip3 install --upgrade setuptools && \
    pip3 install --upgrade wheel && \
    pip3 install pandoc \
    requests==2.21.0

RUN mkdir /logs

RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    echo 'LANG="en_US.UTF-8"'>/etc/default/locale && \
    dpkg-reconfigure --frontend=noninteractive locales && \
    update-locale LANG=en_US.UTF-8

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8
ENV LANG C.UTF-8

RUN echo "downloading and entering grimoirelab-sirmordred" && \
    git clone https://github.com/X-lab2017/grimoirelab-sirmordred.git --depth 1 && \
    cd grimoirelab-sirmordred/ && \
    echo "Installing grimoirelab-sirmordred" && \
    pip3 install -r requirements.txt && \
    python3 setup.py install

ADD ../gitee-default-settings/setup.cfg /setup.cfg
ADD ../gitee-default-settings/projects.json /projects.json
ADD ../gitee-default-settings/menu.yaml /menu.yaml
ADD ../gitee-default-settings/aliases.json /aliases.json

# Entrypoint (mordred)
ENTRYPOINT [ "sirmordred" ]
CMD [ "-c", "/setup.cfg"]
