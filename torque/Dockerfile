FROM agaveapi/torque

RUN trqauthd start && \
    pbs_mom && pbs_sched && pbs_server && \
    # allow services to start up
    sleep 2 && \
    qmgr -c "set server scheduler_iteration = 1"

# Create a user 
RUN /usr/sbin/useradd -p $(openssl passwd alper) alper 
RUN groupmod -g 1000 alper 
RUN usermod -u 1000 alper
RUN usermod -g alper alper

WORKDIR /home/alper

# copy ssh config directory from Docker context to xenon's home directory
COPY .ssh .ssh

RUN chmod 700 .ssh && \
chmod 600 .ssh/id_rsa .ssh/id_dsa && \
chmod 644 .ssh/authorized_keys .ssh/id_rsa.pub .ssh/id_dsa.pub && \
chown -R alper.alper .ssh

ADD docker_entrypoint.sh /docker_entrypoint.sh

# Expose sshd port
EXPOSE 22
