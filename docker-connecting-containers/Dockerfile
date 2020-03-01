FROM python:3.7-slim
RUN apt-get update && apt-get install -y default-libmysqlclient-dev python3-pip wget && \
    pip3 install django==2.2.5 mysqlclient && \
    wget https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh -O /wait-for-it.sh 
RUN mkdir /opt/django && cd /opt/django && \
    django-admin startproject test
wORKDIR /opt/django/test
RUN /bin/bash -c "sed -i 's/ALLOWED_HOSTS\ =\ \[\]/ALLOWED_HOSTS\ =\ \[\"\*\"]/g' test/settings.py" && \
    echo "DATABASES = {'default': {'ENGINE': 'django.db.backends.mysql','NAME': 'db', 'USER': 'django', 'PASSWORD': 'django', 'HOST': 'database', 'PORT': '3306' }}" >> test/settings.py
EXPOSE 8000
CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]
