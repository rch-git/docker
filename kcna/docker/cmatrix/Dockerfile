# Build container image

FROM alpine AS cmatrixbuilder

WORKDIR /cmatrix

RUN	apk update --no-cache && \
		apk add git autoconf automake alpine-sdk ncurses-dev ncurses-static && \
		git clone https://github.com/spurin/cmatrix.git . && \
		mkdir -p /usr/lib/kbd/consolefonts /usr/share/consolefonts&&  \
		autoreconf -i && \
		/cmatrix/configure LDFLAGS="-static" && \
		make
		
# cmatrix Container Image

FROM alpine

LABEL	org.opencontainers.image.authors="RC" \
		org.opencontainers.image.description="Container image for https://github.com/abishekvashok/cmatrix"

RUN	apk update --no-cache && \
		apk add ncurses-terminfo-base sudo && \
		adduser -g "Thomas Anderson" -s /usr/sbin/nologin -D -H thomas && \
		adduser thomas wheel

COPY --from=cmatrixbuilder /cmatrix/cmatrix /cmatrix/cmatrix

USER	thomas
ENTRYPOINT	["/cmatrix/cmatrix"]
CMD	["-M",  "Hello World", "-k"]
