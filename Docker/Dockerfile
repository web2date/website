FROM alpine:3.8

MAINTAINER Daniel Siepmann
LABEL Description="This image should provide environment to render daniel-siepmann.de" Vendor="DanielSiepmann" Version="1.0"

# Define ruby dependencies
ENV RUBY_BUILD_PACKAGES curl-dev libffi-dev ruby-dev build-base
ENV RUBY_PACKAGES ruby ruby-io-console ruby-bundler
ENV TZ=Europe/Berlin

# Install system dependencies
RUN apk update && apk upgrade \
    && apk add bash \
    && apk add pngquant optipng \
    && apk add --update tzdata \
    && apk add python3 \
    && pip3 install --upgrade pip \
    && apk add $RUBY_BUILD_PACKAGES \
    && apk add $RUBY_PACKAGES \
    && rm -rf /var/cache/apk/*

# Install python and ruby dependencies
COPY . /docker
RUN cd /docker \
    && pip3 install --upgrade -r requirements.txt \
    && gem install bundler --no-document \
    && bundle install \
    && rm -rf /docker

# Install TypoScript syntax for pygments
RUN destdir=$(dirname $(python3 -c "import pygments; print(pygments.__file__)"))/lexers \
   && wget https://raw.githubusercontent.com/TYPO3-Documentation/Pygments-TypoScript-Lexer/v2.2.4/typoscript.py --quiet --output-document $destdir/typoscript.py \
   && cd $destdir; python3 _mapping.py
