FROM chpio/gosu:stretch

RUN { set -ex; \
	# add user & group
	groupadd --system --gid 1000 electrum-client; \
	useradd --system --uid 1000 --gid 1000 electrum-client; \

	# install deps
	apt-get update; \
	apt-get install --no-install-recommends -y \
		python3-pyqt5 \
		python3-setuptools \
		python3-requests \
		ca-certificates; \
	rm -rf /var/lib/apt/lists/*; \
}

RUN { set -ex; \
	# install build deps
	BUILD_DEPS="git pyqt5-dev-tools protobuf-compiler"; \
	apt-get update; \
	apt-get install --no-install-recommends -y $BUILD_DEPS; \

	# download
	git clone https://github.com/spesmilo/electrum.git /tmp/build --depth 1 --branch 3.0.1; \
	cd /tmp/build; \
	git checkout d599f11cceb0b74d092befff019097d0f0d3f2ac; \

	# install
	protoc --proto_path=lib/ --python_out=lib/ lib/paymentrequest.proto; \
	pyrcc5 icons.qrc -o gui/qt/icons_rc.py; \
	./contrib/make_locale; \
	python3 setup.py install; \

	# clean up
	apt-get purge -y $BUILD_DEPS; \
	apt-get autoremove -y; \
	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*; \
}

VOLUME /data/electrum-client

COPY start-electrum-client /usr/bin/
ENTRYPOINT ["start-electrum-client"]
