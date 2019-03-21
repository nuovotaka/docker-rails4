FROM ruby:2.6.2
ENV LANG C.UTF-8
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev postgresql-client nodejs

RUN mkdir /myapp
WORKDIR /myapp
ADD Gemfile /myapp/Gemfile
ADD Gemfile.lock /myapp/Gemfile.lock
RUN bundle install
ADD . /myapp
