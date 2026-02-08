FROM debian:trixie-slim

WORKDIR /pbx

RUN apt update
RUN apt upgrade -y
RUN apt install build-essential wget pkg-config libedit-dev uuid-dev libjansson-dev libssl-dev libxml2-dev libsqlite3-dev systemd subversion -y
RUN apt install unixodbc unixodbc-dev libtool libltdl-dev odbc-postgresql -y

COPY ./irc-pbx/asterisk-22-current.tar.gz /usr/src/asterisk-22-current.tar.gz


RUN cd /usr/src && \
    tar -xvzf asterisk-22-current.tar.gz && \
    cd asterisk-22.8.2 && \
    ./contrib/scripts/get_mp3_source.sh && \
    ./contrib/scripts/install_prereq install && \
    ./configure --libdir=/usr/lib64 --with-jansson-bundled
COPY ./irc-pbx/menuselect.makeopts /usr/src/asterisk-22.8.2/menuselect.makeopts

RUN sleep 5 && \
    cd /usr/src/asterisk-22.8.2 && \
    make && \
    make install && \
    make samples && \
    make config  && \
    make clean && \
    cd .. && \
    rm -fr asterisk-22-current.tar.gz
    
#COPY ./menuselect.makeopts /usr/src/asterisk-22.5.2/menuselect.makeopts

#RUN sleep 5 && \

#COPY ./menuselect/menuselect.makeopts /pbx/asterisk-22.5.2/menuselect.makeopts

#RUN cd asterisk-22.5.2 && \
#    make && \
#    make install && \
#    make config && \
#    make clean && \
#    cd .. && \
#    rm -fr asterisk-22-current.tar.gz

ENTRYPOINT ["/usr/sbin/asterisk", "-f"]
#CMD ["start"]