FROM python:3.7
#RUN apt-get update && apt-get install libmysqlclient-dev
RUN pip3 install django==2.2.5 mysqlclient
RUN mkdir /opt/django && cd /opt/django && \
    django-admin startproject test
WORKDIR /opt/django/test
RUN /bin/bash -c "sed -i 's/ALLOWED_HOSTS\ =\ \[\]/ALLOWED_HOSTS\ =\ \[\"\*\"]/g' test/settings.py" && \
    echo "DATABASES = {'default': {'ENGINE': 'django.db.backends.mysql','NAME': 'db', 'USER': 'django', 'PASSWORD': 'django', 'HOST': 'database', 'PORT': '3306' }}" >> test/settings.py
EXPOSE 8000
CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]
