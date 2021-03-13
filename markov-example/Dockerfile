FROM python:3.6-stretch
LABEL maintainer="naomila@uw.edu"
LABEL description="Write a matsci textbook for fun and profit"

# Update the package cache
RUN apt-get update

# Install Python libraries
RUN pip3 install --no-cache-dir markovify

# Install tools to make PDFs
RUN apt-get install -y ghostscript enscript tree

WORKDIR  /usr/src/textbook-writer
COPY src/ ./src/
COPY data/ ./data/

RUN ls -la ./src/*
RUN ls -la ./data/*

ENTRYPOINT ["python3", "./src/main.py"]
CMD ["--sentences", "8"]

