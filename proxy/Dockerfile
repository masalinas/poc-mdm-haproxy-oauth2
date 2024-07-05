FROM haproxy:3.0
MAINTAINER Miguel Salinas Gancedo <masalinas.gancedo@gmail.com> @masalinas

USER root

#RUN apt-get update
#RUN apt-get install libcap2-bin -y
#RUN setcap 'cap_net_bind_service=+ep' /usr/local/sbin/haproxy

#USER haproxy

# defaults for the variables used in haproxy.cfg
COPY haproxy.cfg /usr/local/etc/haproxy/haproxy.cfg
