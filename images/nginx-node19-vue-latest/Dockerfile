FROM node:19-buster

# Add your local user here
RUN useradd -u 1001 mbuelow
RUN mkdir /home/mbuelow
RUN chown mbuelow /home/mbuelow

# APT Repositories aktualisieren
RUN apt update -y

# Wartungstools: vim
RUN apt install -y vim

# Vue installieren
RUN npm install -g @vue/cli

# Nginx installieren
RUN apt install -y nginx

# Nginx konfigurieren
RUN rm /etc/nginx/sites-enabled/default
RUN rm /etc/nginx/sites-available/default

COPY ./config/nginx/nginx.conf /etc/nginx/nginx.conf
COPY ./config/nginx/site.conf /etc/nginx/sites-available/site.conf
RUN ln -s /etc/nginx/sites-available/site.conf /etc/nginx/sites-enabled/site \
&& chmod 0744 /etc/nginx/sites-available/site.conf

# Webserver Root anlegen
RUN mkdir -p /var/www/html/site

# Leeren Ordner zum Anlegen von Projekten anlegen
RUN mkdir -p /creator

# Startup Skript
COPY ./startup.sh /startup.sh
CMD ["/bin/bash", "/startup.sh"]

EXPOSE 80