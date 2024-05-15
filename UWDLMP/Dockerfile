FROM "rocker/r-ubuntu:jammy"
RUN apt-get update
RUN apt-get install -y pandoc libxml2-dev libssl-dev libcurl4-openssl-dev libpq-dev r-cran-cairo r-cran-dbi r-cran-knitr r-cran-lubridate
RUN R -e 'install.packages(c("renv"))'
COPY ./renv.lock .
RUN apt-get install -y r-cran-gtsummary
RUN R -e 'renv::restore()'
RUN R -e 'renv::install("ggplot2@3.4.4")'
WORKDIR /root
COPY ./run .
COPY ./src .
RUN chmod 700 /root/run
ENV TZ=America/Los_Angeles
CMD /root/run
