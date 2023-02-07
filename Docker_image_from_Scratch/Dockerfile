# Create by Andrew Bienvenue
#Creating a CentOS version Docker image from scratch

FROM Kserge2001/centos-ssh
RUN yum update -y
RUN yum install httpd -y
RUN yum install vim -y
RUN service httpd start
EXPOSE 80
#Expose 80, open up port 80
COPY ./index.html /var/www/html
CMD apachectl -D FOREGROUNG
