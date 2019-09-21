### Kubernetes Community Dojo for Docker Presentation
---  
### docker recap

Use all utilities which you learnt during dojo  
Tip: build container with mounted volume and exposed port 8000

`docker run --name app -p 80:8000 greg4fun/django:katacoda`{{execute}}
Access web and check stdout in our console

Run it in detached mode:
`docker run -d --rm --name app -p 80:8000 greg4fun/django:katacoda`{{execute}}

 
---

### Lets try in docker-compose now
