FROM node:8-slim

RUN apt-get update \
	&& apt-get install -y --no-install-recommends python git g++ make chromium libgtk-3-0 libatk-bridge2.0-0 \
	&& rm -rf /var/lib/apt/lists/* && git clone https://github.com/p2bauer/flightplan.git /usr/flightplan
WORKDIR /usr/flightplan
RUN sed -i 's/localhost/flightplanserver/' /usr/flightplan/client/package.json
ENV PATH=/home/node/.npm-global/bin:$PATH
ENV PATH=/usr/flightplan:$PATH
ENV NPM_CONFIG_PREFIX=/home/node/.npm-global
RUN npm install --global -unsafe
RUN apt-get remove -y git g++ make \
	&& apt-get autoclean \
	&& apt-get autoremove -y \
	&& rm -rf /var/lib/apt/lists/*
EXPOSE 3000
ENTRYPOINT ["flightplan","client"]
CMD []
