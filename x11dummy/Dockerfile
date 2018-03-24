FROM debian:stretch
ENV TZ Europe/Moscow
ENV DEBIAN_FRONTEND noninteractive
ENV DISPLAY :1

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN echo "nameserver 8.8.8.8" > /etc/resolv.conf
RUN apt-get update \
    && apt-get -y install apt-utils \
    && apt-get -y install xserver-xorg-video-dummy x11-apps

VOLUME /tmp/.X11-unix

COPY xorg.mini.conf /etc/X11/xorg.conf

CMD /usr/bin/Xorg -noreset +extension GLX +extension RANDR +extension RENDER -logfile /tmp/xdummy.log -config /etc/X11/xorg.conf $DISPLAY