FROM ubuntu:22.04
LABEL autor=Kotoff
LABEL autor=demo
RUN apt-get update \
  && apt-get install -y nginx
ENV OWNER=kotoff
EXPOSE 8080
EXPOSE 443/tcp
CMD ["nginx","-g","daemon off"]