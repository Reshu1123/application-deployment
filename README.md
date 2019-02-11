# application-deployment
#Base image is ubuntu 16

#comment added for test-2

FROM ubuntu

MAINTAINER reshma <rebabu99@in.ibm.com>



USER root

# Install Nginx.

# RUN executes command(s) in a new layer and creates a new image. E.g., it is often used for installing software packages.

RUN \

  apt-get update && \

  apt-get install -y nginx && \

  rm -rf /var/lib/apt/lists/* && \

  echo "\ndaemon off;" >> /etc/nginx/nginx.conf && \

  useradd -G www-data nginx && \

  chown -R nginx:www-data /var/lib/nginx && \

  chown -R nginx:www-data /etc/nginx/ && \

  chown -R nginx:www-data /var/log/nginx && \

  chown -R nginx:www-data /var/www/html && \

  sed -i 's/80/8080/g' /etc/nginx/sites-available/default

# Define mountable directories.

# Set volume mount points for installation and home directory. Changes to thehome directory needs to be persisted as well as parts of the installation directory due to eg. logs.

#VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx", "/var/www/html"]



# Define working directory.

WORKDIR /etc/nginx

RUN sed -i '/pid/s/\ .*/\ \/tmp\/nginx.pid;/' /etc/nginx/nginx.conf

USER nginx

# Define default command.

CMD ["nginx"]



# Expose ports.

EXPOSE 8080

EXPOSE 443 
