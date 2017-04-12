FROM rocker/r-ver:3.3.2

MAINTAINER Oliver Pelz "o.pelz@gmail.com"

#### things we need for compiling R and Perl libraries
#### and another deb pkgs we later need for the R libraries to compile or run
RUN apt-get update && apt-get install -y  \
    wget \
    sudo \
    git \
    libssl-dev 
    
# install the shiny server debian package from r-studio
RUN wget --no-verbose "https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-12.04/x86_64/shiny-server-1.5.3.838-amd64.deb" -O ss-latest.deb && \
    gdebi -n ss-latest.deb && \
    rm -f ss-latest.deb 

COPY ./shiny-server.sh /usr/bin/shiny-server.sh
RUN chmod +x /usr/bin/shiny-server.sh

# now to the R part...


# first we need devtools for all the installation of all further packages
RUN R -e 'install.packages("devtools", repos = "http://cloud.r-project.org/")'

# install all the packages we need from cran, bioconductor and github

# first to install bioconductor R packages
#RUN R -e 'source("http://bioconductor.org/biocLite.R");biocLite("BiocParallel")'
#RUN R -e 'source("http://bioconductor.org/biocLite.R");biocLite("DESeq2")'

# to install R packages from CRAN
#RUN R -e 'devtools::install_version("dplyr", version = "0.5.0", repos = "http://cloud.r-project.org/")'
#RUN R -e 'devtools::install_github("jyyu/ScreenBEAM", ref = "d6204b3")'
#RUN R -e 'devtools::install_version("readr", version = "1.0.0", repos = "http://cloud.r-project.org/")'
#RUN R -e 'devtools::install_version("shinydashboard", version = "0.5.3", repos = "http://cloud.r-project.org/")'

# to install R packages from github
#RUN R -e 'devtools::install_github("jyyu/ScreenBEAM", ref = "d6204b3")'

# cleaning up downloaded deb packages for keeping clean our docker image
RUN apt-get -qq clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# if your shiny app depends on some PERL modules as well

# for CPAN to auto say yes to every question

#ENV PERL_MM_USE_DEFAULT=1
#RUN perl -MCPAN -e 'CPAN::Shell->install("Bundle::CPAN")'

# now install some perl modules we need for xxx
#RUN perl -MCPAN -e 'CPAN::Shell->install("Bio::DB::Fasta")'
#RUN perl -MCPAN -e 'CPAN::Shell->install("Bio::SeqIO")'


# To install some PERL scripts from git
#RUN git clone https://github.com/xxx/yyy.git /tmp/Supplemental-Material
#RUN cp -r /tmp/Supplemental-Material/xxx-runner /opt/
#RUN chmod +x /opt/xxx-runner/start.pl
#env PATH /opt/xxx-runner:$PATH
#RUN rm -rf /tmp/Supplemental-Material
#RUN cd /opt/xxx-runner/depends/Set-IntervalTree-0.10-OD; perl Makefile.PL; make; make test && make install

# install software package bowtie 2 which has a difficult license and therefore cannot be packaged or shipped in a docker image
#COPY ./bowtie2-2.2.9-linux-x86_64.zip /tmp/
#RUN unzip /tmp/bowtie2-2.2.9-linux-x86_64.zip -d /opt
#ENV PATH /opt/bowtie2-2.2.9:$PATH
#RUN echo 'export PATH=/opt/bowtie2-2.2.9/:$PATH' >> /etc/profile

# deploy your shiny app called xxx to docker
COPY ./xxx /srv/shiny-server/xxx/

# we will run the shiny app as user 
RUN chown -R shiny:shiny /srv/shiny-server/xxx

COPY docker-entrypoint.sh /
RUN chmod +x /docker-entrypoint.sh

EXPOSE 3838

ENTRYPOINT ["/docker-entrypoint.sh"]
# finally run
CMD ["start-app"]

