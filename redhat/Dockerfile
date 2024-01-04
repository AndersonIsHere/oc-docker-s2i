FROM ubuntu
USER root
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install apache2 -y && apt-get install apache2-utils -y && apt-get clean
RUN chown 777 /var/run /var/lock
EXPOSE 80
CMD ["apache2ctl","-D","FOREGROUND"]
