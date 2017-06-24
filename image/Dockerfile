FROM chpio/gosu:jessie

RUN { set -ex; \
	# add user & group
	groupadd --system --gid 1000 electrum-client; \
	useradd --system --uid 1000 --gid 1000 electrum-client; \

	# install deps
	apt-get update; \
	apt-get install --no-install-recommends -y python-qt4 python-pip; \
	rm -rf /var/lib/apt/lists/*; \
}

RUN { set -ex; \
	# install
	pip install https://download.electrum.org/2.8.3/Electrum-2.8.3.tar.gz; \

	# clean up
	rm -rf /tmp/* /var/tmp/*; \
}

VOLUME /data/electrum-client

COPY start-electrum-client /usr/bin/
ENTRYPOINT ["start-electrum-client"]
