FROM ubuntu:18.04

RUN apt-get update -y
RUN apt-get upgrade -y

RUN apt-get install wget -y
RUN apt-get install python3.6 -y
RUN apt-get install vim -y
RUN apt-get install git -y

# add docker user
RUN useradd -ms /bin/bash build

USER build
ENV HOME /home/build
WORKDIR /home/build

# download conda
RUN ["/bin/bash", "-c", "wget http://repo.continuum.io/miniconda/Miniconda-latest-Linux-x86_64.sh -O $HOME/miniconda.sh"]
RUN chmod 0755 $HOME/miniconda.sh
RUN ["/bin/bash", "-c", "$HOME/miniconda.sh -b -p $HOME/conda"]
ENV PATH="$HOME/conda/bin:$PATH"
RUN rm $HOME/miniconda.sh

# update conda
RUN conda update conda
RUN conda install conda-build
RUN conda install binstar
RUN conda install pandas
RUN conda install seaborn
RUN conda install -c anaconda python=3.6
RUN conda install --yes pytorch=0.3.1 torchvision cuda80 -c pytorch

USER root
# Frame extracting support code
RUN wget http://johnvansickle.com/ffmpeg/releases/ffmpeg-4.1.3-amd64-static.tar.xz
RUN tar xvf ffmpeg-4.1.3-amd64-static.tar.xz
RUN cp ffmpeg-4.1.3-amd64-static/ffmpeg /usr/local/bin
RUN cp ffmpeg-4.1.3-amd64-static/ffprobe /usr/local/bin


# Copy over resnets code from git repo to the image
RUN git clone https://github.com/akashp1196/aolme_resnets.git
RUN cp -r aolme_resnets/ /app/
RUN cp aolme_resnets/extras/* /usr/local/bin/

USER build
WORKDIR /
