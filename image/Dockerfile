FROM chpio/gosu:stretch

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
	# install build deps
	BUILD_DEPS="python-setuptools"; \
	apt-get update; \
	apt-get install --no-install-recommends -y $BUILD_DEPS; \



	# install
	pip install https://download.electrum.org/3.0.1/Electrum-3.0.1.tar.gz; \

	# clean up
	apt-get purge -y $BUILD_DEPS; \
	apt-get autoremove -y; \
	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*; \
}

VOLUME /data/electrum-client

COPY start-electrum-client /usr/bin/
ENTRYPOINT ["start-electrum-client"]
