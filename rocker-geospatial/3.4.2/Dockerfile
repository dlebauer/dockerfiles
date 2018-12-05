FROM rocker/geospatial:3.5.1
MAINTAINER "Tyson Lee Swetnam" tswetnam@cyverse.org

# Original from Terra-Ref https://github.com/terraref/toolserver-dockerfiles/blob/master/rstudio/Dockerfile

## VICE Depends
RUN apt-get update \
    && apt-get install -y lsb wget apt-transport-https python2.7 python-requests curl supervisor nginx gnupg2

RUN wget -qO - https://packages.irods.org/irods-signing-key.asc | apt-key add - \
    && echo "deb [arch=amd64] https://packages.irods.org/apt/ xenial main" > /etc/apt/sources.list.d/renci-irods.list \
    && apt-get update \
	&& apt-get install -y irods-icommands

ADD https://github.com/hairyhenderson/gomplate/releases/download/v2.5.0/gomplate_linux-amd64 /usr/bin/gomplate
RUN chmod a+x /usr/bin/gomplate

# Rocker Geospatial RStudio Depends
RUN apt-get update -y &&  \
    apt-get install -y -qq \
         cron \
         emacs-nox \
         imagemagick \
         python2.7-dev \
         python3-dev \
         python3-pip \
         unixodbc-dev \
         odbc-postgresql \
         sqlite3 \
         libopencv-dev \
         libsqliteodbc \
         python-matplotlib  && \
    easy_install pip && \
    pip install \
         numpy \
         scipy \
         matplotlib \
         ipython \
         jupyter \
         pandas \
         sympy \
         nose \
         scikit-image \
         opencv-python  && \
    pip3 install \
         numpy \
         scipy \
         matplotlib  && \
     apt-get clean && \
     rm -rf /var/lib/apt/lists/*

RUN install2.r --error \
    GGally \
    RCurl \
    convertr \
    ggthemes \
    ncdf.tools \
    ncdf4 \
    rLiDAR \
    leaflet \
    shinythemes 
RUN R -e "devtools::install_github('terraref/traits')"
RUN R -e "devtools::install_github('rstats-db/odbc')"
RUN R -e "devtools::install_github('rstudio/blogdown')"
RUN Rscript -e 'source("http://bioconductor.org/biocLite.R")' -e 'biocLite("EBImage")'

# Install plantcv and dependencies, only supported under Python 2.x
RUN  pip install -U six opencv-python && cd / &&  git clone https://github.com/danforthcenter/plantcv.git && \
      cd /plantcv && python setup.py install
   
USER rstudio

USER root
RUN echo "\n\n if [ ! -d /home/\$USER/tutorials ]; then\n\t git clone https://github.com/terraref/tutorials.git /home/\$USER/tutorials\nfi\n" >> /etc/cont-init.d/userconf && \
    echo "\nchown -R \$USER /home/\$USER/tutorials" >> /etc/cont-init.d/userconf  && \
    echo "\n if [ ! -d /home\$USER/data ]; then\n\tln -s /data/ /home/\$USER/data\nfi\n" >> /etc/cont-init.d/userconf
    
## VICE Configs
COPY run.sh /usr/local/bin/run.sh
COPY nginx.conf.tmpl /nginx.conf.tmpl
COPY rserver.conf /etc/rstudio/rserver.conf
COPY supervisor-nginx.conf /etc/supervisor/conf.d/nginx.conf
COPY supervisor-rstudio.conf /etc/supervisor/conf.d/rstudio.conf

ENV REDIRECT_URL "http://localhost/"

ENTRYPOINT ["/usr/local/bin/run.sh"]    
