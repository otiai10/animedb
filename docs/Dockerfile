FROM node:26.13.1-bookworm AS node
FROM ruby:2.7.8-bullseye

ENV LANG=C.UTF-8

COPY --from=node /usr/local/bin/node /usr/local/bin/node
COPY --from=node /usr/local/lib/node_modules/npm /usr/local/lib/node_modules/npm
RUN ln -s /usr/local/lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm

#####
# Install dependencies and codes
#

ENV INSTALL_PATH=/mnt/docs

WORKDIR $INSTALL_PATH

COPY Gemfile Gemfile
COPY Gemfile.lock Gemfile.lock
RUN gem install bundler -v 2.4.22 --no-document && bundle _2.4.22_ install

COPY package.json package.json
RUN npm install
RUN mv node_modules /

COPY . .
RUN rm -rf node_modules && mv /node_modules .

RUN npm run build

EXPOSE 4000

CMD ["bundle", "exec", "jekyll", "serve", "--host=0.0.0.0"]
