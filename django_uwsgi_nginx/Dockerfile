
FROM jmrose/nginx-python
MAINTAINER wkdal001@naver.com

ENV LANG C.UTF-8

# COPY requirements.txt and RUN pip install BEFORE adding the rest of your code, this will cause Docker's caching mechanism
# to prevent re-installing (all your) dependencies when you made a change a line or two in your app.

COPY app/requirements.txt /home/docker/code/app/
RUN /root/.pyenv/versions/app/bin/pip install -r \
/home/docker/code/app/requirements.txt

# add (the rest of) our code
COPY . /home/docker/code/

# pyenv local설정
WORKDIR /home/docker/code/app
RUN pyenv local app

# setup all the configfiles
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
COPY nginx-app.conf /etc/nginx/sites-available/default
COPY supervisor-app.conf /etc/supervisor/conf.d/

# uWSGI Log
RUN mkdir -p /var/log/uwsgi/app

EXPOSE 80
CMD ["supervisord", "-n"]
