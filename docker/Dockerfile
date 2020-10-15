FROM nginx
RUN apt-get update && apt-get -y install fortune
RUN ln -s /usr/games/fortune /usr/bin/fortune
CMD tail -f /dev/null

