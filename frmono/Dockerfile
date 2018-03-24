FROM debian:stretch
ENV TZ Europe/Moscow
ENV DEBIAN_FRONTEND noninteractive
ENV DISPLAY :1

WORKDIR /

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN echo "nameserver 8.8.8.8" > /etc/resolv.conf

# Exit code 1. WTF? Docker bug?
#RUN mkdir –p /opt/fastreport/htdocs/bin
RUN ["mkdir", "-p", "/opt/fastreport/htdocs/bin"]

RUN apt-get update \
     && apt-get -y install apt-utils \
     && apt-get -y install unzip \
     && apt-get -y install mono-complete mono-xsp apache2 libapache2-mod-mono

# Apache setup    
COPY 001-mono.conf /etc/apache2/sites-available/001-mono.conf
RUN /usr/sbin/update-rc.d apache2 disable \
	&& /usr/sbin/a2dismod mod_mono_auto \
	&& /usr/sbin/a2dissite 000-default \
	&& /usr/sbin/a2ensite 001-mono 

# FastReport.Mono setup	
# Download official Demo 
ADD https://www.fastreport.ru/public_download/frmono_demo.zip /tmp/frmono_demo.zip

# Or add from local filesystem
#COPY frmono_demo.zip /tmp/frmono_demo.zip

# Extract and copy to destinations
RUN	["unzip", "/tmp/frmono_demo.zip", "-d", "/tmp/frmono.demo"]
RUN cp -rp /tmp/frmono.demo/Demos/C#/Web/* /opt/fastreport/htdocs
RUN cp /tmp/frmono.demo/FastReport.*.dll /opt/fastreport/htdocs/bin
RUN chown -R www-data:www-data /opt/fastreport
# Volume for X11 unix socket
VOLUME /tmp/.X11-unix

# Start Apache in foreground to prevent container exit
CMD ["/usr/sbin/apachectl", "-DFOREGROUND"]