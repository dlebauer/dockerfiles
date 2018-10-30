FROM rocker/ropensci:latest

MAINTAINER "Tyson Lee Swetnam" tswetnam@cyverse.org

## VICE Depends
RUN apt-get update \
    && apt-get install -y lsb wget apt-transport-https python2.7 python-requests curl supervisor nginx gnupg2

RUN wget -qO - https://packages.irods.org/irods-signing-key.asc | apt-key add - \
    && echo "deb [arch=amd64] https://packages.irods.org/apt/ xenial main" > /etc/apt/sources.list.d/renci-irods.list \
    && apt-get update \
	&& apt-get install -y irods-icommands

ADD https://github.com/hairyhenderson/gomplate/releases/download/v2.5.0/gomplate_linux-amd64 /usr/bin/gomplate
RUN chmod a+x /usr/bin/gomplate

## Eddy4R Depends
RUN apt-get update
RUN apt-get install -y \
    # EBImage
      fftw3 \
      fftw3-dev \
      libjpeg-dev \
      libtiff-dev \
    # REddyProc
      libudunits2-0 \
      libudunits2-dev \
      libnetcdf-dev \
      udunits-bin

# eddy4R-Docker example workflow and data

  # create directory
RUN mkdir -p /home/eddy
  
  # provide read and write access to web-hosted installation script
RUN chmod -R 777 /home/eddy

# install the eddy4R packages via web-hosted installation script
RUN R -e 'source("https://www.dropbox.com/s/xmgsctjbrekfyw8/flow.inst.eddy4r.R?dl=1")'

  # provide read and write access to Rstudio users for example workflow and data location
RUN chmod -R 777 /home/eddy

# provide read and write access to Rstudio users for default R library location
RUN chmod -R 777 /usr/local/lib/R/site-library

## VICE Configs
COPY run.sh /usr/local/bin/run.sh
RUN chmod +x /usr/local/bin/run.sh

COPY nginx.conf.tmpl /nginx.conf.tmpl
COPY rserver.conf /etc/rstudio/rserver.conf
COPY supervisor-nginx.conf /etc/supervisor/conf.d/nginx.conf
COPY supervisor-rstudio.conf /etc/supervisor/conf.d/rstudio.conf

ENV REDIRECT_URL "http://localhost/"

ENTRYPOINT ["/usr/local/bin/run.sh"]
