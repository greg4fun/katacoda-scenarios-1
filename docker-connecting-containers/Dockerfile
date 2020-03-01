FROM ubuntu:18.04
RUN apt-get update && \
    apt-get install -y libmysqlclient-dev python3-pip && \
    pip3 install django==2.2.5 mysqlclient
RUN mkdir /opt/django && cd /opt/django && \
    django-admin startproject testapp
WORKDIR /opt/django/testapp
RUN /bin/bash -c "sed -i 's/ALLOWED_HOSTS\ =\ \[\]/ALLOWED_HOSTS\ =\ \[\"\*\"]/g' testapp/settings.py" && \
    echo "DATABASES = {'default': {'ENGINE': 'django.db.backends.mysql','NAME': 'db', 'USER': 'django', 'PASSWORD': 'django', 'HOST': 'database', 'PORT': '3306' }}" >> testapp/settings.py
EXPOSE 8000
CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]
