FROM debian:latest

#  $ docker build . -t continuumio/anaconda3:latest -t continuumio/anaconda3:5.3.0
#  $ docker run --rm -it continuumio/anaconda3:latest /bin/bash
#  $ docker push continuumio/anaconda3:latest
#  $ docker push continuumio/anaconda3:5.3.0

ENV LANG=en_US.UTF-8 LC_ALL=en_US.UTF-8 TERM=xterm 
ENV PATH /opt/conda/bin:$PATH

RUN apt-get update --fix-missing && apt-get install -y wget bzip2 ca-certificates \
    libglib2.0-0 libxext6 libsm6 libxrender1 \
    git mercurial subversion && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN wget --quiet https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc

RUN conda update --all -y && \
    conda clean --all -y

RUN conda install -c anaconda seaborn && \
    conda clean --all -y

RUN conda install pytorch torchvision cpuonly -c pytorch && \
    conda clean --all -y

RUN conda install -c pytorch -c fastai fastai && \
    conda clean --all -y

RUN conda install -c conda-forge jupyter_contrib_nbextensions jupyter_nbextensions_configurator jupyterthemes && \
    jupyter nbextensions_configurator enable && \
    conda clean --all -y

RUN conda install nb_conda && \
    conda clean --all -y

RUN conda install -c conda-forge twine && \
    conda clean --all -y

RUN pip install --no-cache-dir nbdev
    

RUN mkdir /opt/notebooks

VOLUME [ "/opt/notebooks" ]

CMD [ "/opt/conda/bin/jupyter", "lab", "--notebook-dir=/opt/notebooks", "--ip='*'", "--port=8888", "--no-browser", "--allow-root" ]
