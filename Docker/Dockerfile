FROM ubuntu:22.04
LABEL autor=Kotoff
LABEL autor=demo
RUN apt-get update \
  && apt-get install -y nginx   
CMD ["nginx","-g","daemon off"]
EXPOSE 8080

