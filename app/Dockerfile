FROM python:3.8-alpine3.11


# set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1


# set work directory
WORKDIR /usr/src/app

# ## Build Postgis and dependencies
# postgresql-client is required by psql
# postgresql-dev musl-dev gcc are required by psycopg
# NOTE: there is py3-psycopg2
# libxml2-dev libxslt-dev are required by lxml
# gdal-dev geos-dev proj4-dev are required by geodjango
# libcrypto1.1 is required by gdal
# NOTE: we actually need gdal-dev not gdal
# linux-headers is required by uwsgi
# gettext-dev is required by ./manage.py makemessages

# TODO: optimize installation by using --virtual
RUN apk update && apk upgrade \
    && apk add postgresql-client \
        postgresql-dev \
        musl-dev \
        gcc \
        libxml2-dev \
        libxslt-dev \
        linux-headers \
        gettext-dev \
    && apk add --repository http://dl-cdn.alpinelinux.org/alpine/edge/main \
        libcrypto1.1 \
    && apk add --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing \
        gdal-dev \
        geos-dev \
    && pip install pipenv


WORKDIR /usr/src/app

# install dependencies
RUN pip install --upgrade pip
COPY ./requirements.txt /usr/src/app/requirements.txt
RUN pip install -r requirements.txt

# copy entrypoint.sh
COPY ./entrypoint.sh /usr/src/app/entrypoint.sh

# copy project
COPY . /usr/src/app/

# run entrypoint.sh
ENTRYPOINT ["/usr/src/app/entrypoint.sh"]




