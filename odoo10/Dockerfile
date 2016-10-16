FROM ubuntu:16.04
MAINTAINER Cesar Barron <cesar.barron@gmail.com>

RUN echo 'APT::Get::Assume-Yes "true";' >> /etc/apt/apt.conf \
    && echo 'APT::Get::force-yes "true";' >> /etc/apt/apt.conf
RUN locale-gen fr_FR \
    && locale-gen en_US.UTF-8 \
    && dpkg-reconfigure locales \
    && update-locale LANG=en_US.UTF-8 \
    && update-locale LC_ALL=en_US.UTF-8
RUN ln -s /usr/share/i18n/SUPPORTED /var/lib/locales/supported.d/all \
    && locale-gen
ENV PYTHONIOENCODING utf-8
ENV TERM xterm
RUN apt-get update -q && apt-get upgrade -q && \
    apt-get install --allow-unauthenticated -q \
    fontconfig \
    python-dateutil \
    python-docutils \
    python-feedparser \
    python-jinja2 \
    python-ldap \
    python-libxslt1 \
    python-lxml \
    python-mako \
    python-mock \
    python-openid \
    python-psycopg2 \
    python-pybabel \
    python-pychart \
    python-pyparsing \
    python-reportlab \
    python-simplejson \
    python-tz \
    python-unittest2 \
    python-vatnumber \
    python-vobject \
    python-webdav \
    python-werkzeug \
    python-xlwt \
    python-pip \
    python-pypdf \
    python-passlib \
    python-decorator \
    gcc \
    python-dev \
    mc \
    bzr \
    python-setuptools \
    python-markupsafe \
    python-reportlab-accel \
    python-zsi \
    python-yaml \
    python-argparse \
    python-openssl \
    python-egenix-mxdatetime \
    python-usb \
    python-serial \
    lptools \
    make \
    python-pydot \
    python-psutil \
    python-paramiko \
    poppler-utils \
    python-pdftools \
    antiword \
    python-requests \
    python-xlsxwriter \
    python-suds \
    python-psycogreen \
    python-ofxparse \
    python-gevent \
    nano \
    qrencode \
    wget \
    xmlstarlet \
    xsltproc \
    zbar-tools \
    fontconfig \
    libevent-dev \
    libjpeg-dev \
    libldap2-dev \
    libsasl2-dev \
    libx11-6 \
    libxext6 \
    libxml2-dev \
    libxrender1 \
    libxslt-dev \
    npm \
    python \
    xfonts-75dpi \
    xfonts-base \
    ghostscript \
    graphviz \
    python-babel \
    python-imaging \
    python-matplotlib \
    python-soappy \
    python-software-properties \
    python-zbar

RUN ln -s /usr/bin/nodejs /usr/bin/node
RUN npm install -g less less-plugin-clean-css

RUN echo 'deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main' >> /etc/apt/sources.list.d/pgdg.list && \
    wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | \
    sudo apt-key add -
RUN apt-get update -q && apt-get upgrade -q && \
    apt-get install --allow-unauthenticated -q \
    postgresql-9.6

RUN cd /tmp && wget -O wkhtmltox-0.12.1_linux-trusty-amd64.deb http://download.gna.org/wkhtmltopdf/0.12/0.12.2.1/wkhtmltox-0.12.2.1_linux-trusty-amd64.deb \
    && dpkg -i wkhtmltox-0.12.1_linux-trusty-amd64.deb

RUN cd /tmp && wget -O gdata-2.0.18.tar.gz https://pypi.python.org/packages/a8/70/bd554151443fe9e89d9a934a7891aaffc63b9cb5c7d608972919a002c03c/gdata-2.0.18.tar.gz \
    && tar zxvf gdata-2.0.18.tar.gz \
    && chown -R odoo: gdata-2.0.18 \
    && -s

RUN cd /tmp/gdata-2.0.18 && python setup.py install

RUN cd /tmp && wget -q https://raw.githubusercontent.com/pypa/pip/master/contrib/get-pip.py && python get-pip.py
RUN cd /tmp && wget -q https://raw.githubusercontent.com/odoo/odoo/10.0/requirements.txt && pip install -r requirements.txt
RUN apt-get clean && rm -rf /var/lib/apt/lists/* && rm -rf /tmp/*

RUN adduser --home=/home/odoo --disabled-password --gecos "" --shell=/bin/bash odoo
RUN echo 'root:odoo' |chpasswd

ADD files/entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ADD files/logrotate /etc/logrotate.d/odoo-server
RUN chmod 755 /etc/logrotate.d/odoo-server

RUN mkdir -p /var/log/odoo && \
    chown odoo:root /var/log/odoo

VOLUME ["/home/odoo/", "/var/log/odoo"]

USER ODOO
CMD /entrypoint.sh

EXPOSE 8069
EXPOSE 8072