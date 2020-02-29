FROM	debian:stable	AS build
RUN	apt-get update							&& \
	apt-get upgrade -V --yes					&& \
	apt-get install -V \
			gcc \
			make \
			git \
			pkg-config \
			libbsd-dev \
			deborphan \
			--yes						&& \
	apt-get autoremove --purge --yes				&& \
	apt-get purge $(deborphan) --yes				&& \
	apt-get autoclean						&& \
	apt-get clean
WORKDIR	/tmp
RUN	git clone https://github.com/alejandro-colomar/libalx.git	&& \
	make	base telnet-tcp			-C libalx	-j 8	&& \
	make	install-base install-telnet-tcp	-C libalx		&& \
	rm -rf	libalx
RUN	git clone --single-branch --branch rob \
		https://github.com/alejandro-colomar/rob_cam.git	&& \
	make	-C rob_cam/rob/	-j 2

FROM	debian:stable
WORKDIR	/app
RUN	apt-get update							&& \
	apt-get upgrade --yes						&& \
	apt-get install -V \
			telnet \
			--yes						&& \
	apt-get autoremove --purge --yes				&& \
	apt-get autoclean						&& \
	apt-get clean
COPY	--from=build /tmp/rob_cam/rob/rob ./
RUN	chmod +x ./rob
CMD	["./rob"]