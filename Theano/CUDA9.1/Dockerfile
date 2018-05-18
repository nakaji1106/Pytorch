FROM nvidia/cuda-ppc64le:9.1-cudnn7-devel-ubuntu16.04
MAINTAINER Yasuhiro Nakajima <e35294@jp.ibm.com>

# install base
RUN apt-get update && DEBIAN_FRONTEND=noninteractive  apt-get install -y \
    wget curl git apt-transport-https apt-utils vim htop tree zip unzip \
    openssh-server iputils-ping net-tools language-pack-ja-base language-pack-ja fonts-ipafont-mincho build-essential openjdk-8-jdk  sudo&& \
    apt-get clean

# upgrade module
RUN DEBIAN_FRONTEND=noninteractive apt-get upgrade -y && \
    apt-get clean

# 1. ------------ Installing Anaconda3  ------------------ 

COPY response /tmp/

RUN cd /tmp && \
    wget https://repo.anaconda.com/archive/Anaconda3-5.1.0-Linux-ppc64le.sh &&\
    bash Anaconda3-5.1.0-Linux-ppc64le.sh < /tmp/response

RUN echo "export PATH=/root/anaconda3/bin/:$PATH" >> /root/.bashrc
ENV PATH=/root/anaconda3/bin/:$PATH


# import python modules
RUN pip install IPython==5.3 &&\
    pip install jupyter &&\
    mkdir /root/.jupyter &&\
    echo "c.NotebookApp.ip = '*'" > ~/.jupyter/jupyter_notebook_config.py

# 2. ------------ Installing Keras and Theano  ------------------ 

RUN pip install Keras &&\
    pip install Theano

# 3. ------------ Keras : Switching to the Theano backend  ------------------

RUN mkdir /root/.keras

COPY keras.json /root/.keras

# 4. ------------ install nccl  ------------------

RUN cd / && \
   git clone https://github.com/NVIDIA/nccl.git && \
   cd /nccl  && \
   make CUDA_HOME=/usr/local/cuda test 
   
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/nccl/build/lib

#     ------------------------ END --------------------------------

# setup ssh
RUN mkdir /var/run/sshd
RUN echo root:$(echo "passw0rd") | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -i '/^AcceptEnv/s/^/#/' /etc/ssh/sshd_config
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
RUN echo "export PATH=/usr/local/nvidia/bin:/usr/local/cuda/bin:$PATH" >> /root/.bashrc && \
    echo "export LD_LIBRARY_PATH=/usr/local/nvidia/lib:/usr/local/nvidia/lib64:/nccl/build/lib" >> /root/.bashrc

# setup timezone and locale
ENV TZ=Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN echo "export LANG=ja_JP.UTF-8" >> /root/.bashrc
RUN echo "export LANGUAGE=ja_JP.UTF-8" >> /root/.bashrc
ENV LANGUAGE=ja_JP
ENV LANG=ja_JP.UTF-8

RUN rm -f /tmp/response
RUN rm -f /tmp/Anaconda*

CMD []
