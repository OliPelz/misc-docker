FROM rocker/r-ver:3.3.2

MAINTAINER Oliver Pelz "o.pelz@dkfz.de"

RUN \
    apt-get update -y && \
    apt-get install git curl -y

# install  RUST compiler with mapper and extractor components
RUN \
    curl https://sh.rustup.rs > /tmp/sh.rustup.rs && \
    chmod +x /tmp/sh.rustup.rs && \
    /tmp/sh.rustup.rs -y && \
rm /tmp/sh.rustup.rs

ENV PATH=/root/.cargo/bin:$PATH

# compile CRISPRAnalyzer mapper and extractor
RUN \
    git clone https://github.com/OliPelz/fastq_extractor_proof_of_principle.git \
    /tmp/crispranalyzer-rust-tools
# target dir for compiled executables
RUN \
    mkdir -p /opt/crispranalyzer-tools

# source cargo env file so we can use it
RUN \
    cd /tmp/crispranalyzer-rust-tools/extractor_in_RUST/fastq_parser && \
    cargo build --release && \
    cp ./target/release/fastq_parser /opt/crispranalyzer-tools/ && \
    chmod +x /opt/crispranalyzer-tools/fastq_parser

RUN \
   cd /tmp/crispranalyzer-rust-tools/sam_mapper_in_RUST/sam_mapper/ && \
   cargo build --release && \
   cp ./target/release/sam_mapper /opt/crispranalyzer-tools/ && \
   chmod +x /opt/crispranalyzer-tools/sam_mapper


ENV PATH=/opt/crispranalyzer-tools:$PATH
RUN echo 'export PATH=/opt/crispranalyzer-tools/:$PATH' >> /etc/profile




