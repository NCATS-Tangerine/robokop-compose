FROM python:3.7.3-alpine

WORKDIR /

RUN apk add git
RUN pip install --upgrade pip
COPY requirements.txt ./
RUN pip install --no-cache-dir -r ./requirements.txt
RUN git clone https://github.com/NCATS-Tangerine/tranql.git
RUN apk del git

ENV PYTHONPATH /tranql
ENV WORKERS=2
ENV APP_MODULE=tranql.backplane.server:app
ENV APP_NAME=backplane
ENV PORT=8099

# This is the only variable currently needed only by the app service.
# Including it here to avoid a superfluous additional layer.
ENV BACKPLANE=http://tranql-backplane.renci.org

ENTRYPOINT /usr/local/bin/gunicorn --workers=$WORKERS --bind=0.0.0.0:$PORT --name=$APP_NAME --timeout=600 $APP_MODULE
