FROM ruby:2.7.1
ENV LANG C.UTF-8
ENV APP_ROOT /usr/src/app
RUN apt-get update -qq && \
    apt-get install -y vim nodejs \
                       mariadb-client \
                       --no-install-recommends && \
    rm -rf /var/lib/apt/lists/* /var/cache/apt/archives/* /tmp/* /var/tmp/*



RUN apt-get install -y curl unzip

RUN curl "https://d1vvhvl2y92vvt.cloudfront.net/awscli-exe-linux-aarch64.zip" -o "awscliv2.zip"
RUN unzip awscliv2.zip
RUN ./aws/install


RUN bash -c 'echo complete -C '/usr/bin/aws_completer' aws  >> $HOME/.bashrc'

# install command.
RUN apt-get update && apt-get install -y less vim wget unzip

# install terraform.
# https://azukipochette.hatenablog.com/entry/2018/06/24/004354
RUN wget https://releases.hashicorp.com/terraform/0.12.28/terraform_0.12.28_linux_arm.zip && \
    unzip ./terraform_0.12.28_linux_arm.zip -d /usr/local/bin/

# create workspace.

# initialize command.
ARG AWS_ACCESS_KEY_ID
ARG AWS_SECRET_ACCESS_KEY
ARG AWS_REGION
COPY ./.aws /root/.aws
COPY ./init.sh /root/init.sh
RUN chmod +x /root/init.sh
RUN /bin/bash -c "source /root/init.sh"

WORKDIR /root/src



RUN apt-get update
RUN apt-get -y install apt-transport-https ca-certificates curl gnupg2 software-properties-common
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
RUN add-apt-repository "deb [arch=arm64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"

RUN apt-get update
RUN apt-get -y install docker-ce
RUN apt-get install -yy less

RUN curl -L https://github.com/docker/compose/releases/download/1.25.3/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
RUN chmod +x /usr/local/bin/docker-compose

RUN gem install net-ssh
RUN gem install net-scp


CMD DOCKER_OPTS='-H tcp://0.0.0.0:4243 -H unix:///var/run/docker.sock' && service docker start