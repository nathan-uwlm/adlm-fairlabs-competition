FROM "rocker/r-ubuntu:focal"
RUN apt-get update
RUN apt-get install -y pandoc libxml2-dev libssl-dev libcurl4-openssl-dev libpq-dev r-cran-cairo r-cran-dbi r-cran-knitr r-cran-lubridate
RUN R -e 'install.packages(c("renv"))'
COPY ./renv.lock .
RUN R -e 'renv::restore()'
WORKDIR /root
COPY ./run .
COPY ./src .
RUN chmod 700 /root/run
ENV TZ=America/Los_Angeles
CMD /root/run
