# Putting it in to Practice

`docker build -t kubedojo/webapp .`{{execute}}

`docker images`{{execute}}

`docker run -p 8888:5000 --name wiley_webapp kubedojo/webapp`{{execute}}
