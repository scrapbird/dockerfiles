FROM ubuntu:16.04

RUN apt-get update -y 
RUN apt-get install gcc python3-dev python3-pip libssl-dev swig libffi-dev ssdeep libfuzzy-dev git -y

WORKDIR /opt
RUN git clone --depth=1 https://github.com/viper-framework/viper
WORKDIR /opt/viper
RUN pip3 install -r requirements.txt
RUN git submodule init && git submodule update && make install

# Add user
RUN adduser -S viper
RUN chown -r viper:viper /opt/viper
USER viper

CMD ["viper-cli"]
