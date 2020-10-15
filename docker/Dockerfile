FROM nginx
RUN apt-get update && apt-get -y install fortune && apt-get -y install cowsay
RUN ln -s /usr/games/fortune /usr/bin/fortune && ln -s /usr/games/cowsay /usr/bin/cowsay
CMD tail -f /dev/null

