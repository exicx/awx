ARG TAG=latest
FROM ansible/awx_devel:${TAG}

RUN yum install -y gcc python36-devel
RUN python3 -m ensurepip && pip3 install virtualenv ansible-runner
RUN yum remove -y gcc python36-devel && rm -rf /var/cache/yum

RUN rm -f /etc/ssh/ssh_host_ecdsa_key /etc/ssh/ssh_host_rsa_key
RUN ssh-keygen -q -N "" -t dsa -f /etc/ssh/ssh_host_ecdsa_key
RUN ssh-keygen -q -N "" -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN sed -i "s/#UsePrivilegeSeparation.*/UsePrivilegeSeparation no/g" /etc/ssh/sshd_config
RUN sed -i "s/UsePAM.*/UsePAM yes/g" /etc/ssh/sshd_config
RUN sed -i "s/#StrictModes.*/StrictModes no/g" /etc/ssh/sshd_config
RUN mkdir -p /root/.ssh
RUN ln -s /awx_devel/authorized_keys /root/.ssh/authorized_keys

ENTRYPOINT ["tini", "--"]
CMD ["/usr/sbin/sshd", "-D"]

EXPOSE 22
