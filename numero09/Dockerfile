FROM httpd:2.4.63
RUN apt-get update && apt-get install -y unzip && rm -rf /var/lib/apt/lists/*
COPY material-kit-master.zip /tmp/
RUN unzip /tmp/material-kit-master.zip -d /usr/local/apache2/htdocs/
RUN mv /usr/local/apache2/htdocs/material-kit-master/* /usr/local/apache2/htdocs/
RUN rm -rf /usr/local/apache2/htdocs/material-kit-master
EXPOSE 80