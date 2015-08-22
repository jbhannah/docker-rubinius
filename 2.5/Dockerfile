FROM phusion/baseimage:0.9.17

ENV RBX_DEPENDENCIES bison ruby2.0 ruby2.0-dev rake zlib1g-dev libyaml-dev \
      libssl-dev libreadline-dev libncurses5-dev llvm llvm-dev libeditline-dev \
      libedit-dev
# Fix for Ruby 2.0 on Trusty from:
# https://bugs.launchpad.net/ubuntu/+source/ruby2.0/+bug/1310292/comments/25
RUN apt-get update && \
    apt-get install -y build-essential libyaml-0-2 $RBX_DEPENDENCIES && \
    apt-get clean && \
    dpkg-divert --add --rename --divert /usr/bin/ruby.divert /usr/bin/ruby && \
    dpkg-divert --add --rename --divert /usr/bin/gem.divert /usr/bin/gem && \
    update-alternatives --install /usr/bin/ruby ruby /usr/bin/ruby2.0 1 && \
    update-alternatives --install /usr/bin/gem gem /usr/bin/gem2.0 1 && \
    gem install bundler --no-rdoc --no-ri

RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

ENV RBX_VERSION 2.5.8
RUN curl https://s3.amazonaws.com/releases.rubini.us/rubinius-$RBX_VERSION.tar.bz2 | tar xj && \
    cd rubinius-$RBX_VERSION && \
    bundle install && \
    ./configure --prefix=/opt/rubies/rubinius-$RBX_VERSION && \
    rake build && \
    sudo rake install && \
    cd .. && rm -rf rubinius-$RBX_VERSION

ENV PATH /opt/rubies/rubinius-$RBX_VERSION/bin:$PATH
RUN rm -rf /tmp/* /var/tmp/* && \
    apt-get --purge -y autoremove $RBX_DEPENDENCIES && \
    apt-get clean
