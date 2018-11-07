FROM rocker/shiny:latest

# Install Dependencies
RUN chmod a+rwx /usr/local/lib/R/site-library
RUN apt-get update && apt-get install -y curl git libboost-all-dev libgdal-dev libgdal20 libgit2-dev libproj-dev libssl-dev  libudunits2-0 libudunits2-dev
RUN R -e "install.packages(c('curl', 'devtools', 'digest', 'htmltools', 'httpuv', 'nneo', 'plyr', 'Rcpp', 'reshape2', 'scales', 'sf', 'stringi', 'units', 'yaml'))"
RUN R -e "install.packages(c('mime', 'glue', 'geosphere', 'jsonlite', 'leaflet', 'dplyr', 'DT', 'markdown', 'rgdal', 'tidyr'))"
RUN R -e "install.packages(c('shinythemes', 'shinyWidgets', 'shinyBS', 'shinyjs', 'spocc', 'leaflet.extras','wallace'))"
RUN chmod -R a+rwx /usr/local/lib/R/site-library

# Remove Shiny example inherited from the base image
RUN rm -rf /srv/shiny-server/*

RUN cd /srv/shiny-server/

# change permission of the shiny folder where the app sits
RUN chmod -R 777 /srv/shiny-server

# Start the server with the container
CMD ["/usr/bin/shiny-server.sh"]
