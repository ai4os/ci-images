FROM ai4oshub/ci-images:base-u18
LABEL org.opencontainers.image.authors='Pablo Orviz <orviz@ifca.unican.es>, Valentin Kozlov <valentin.kozlov@kit.edu>'

RUN apt-get update \
    && apt-get install --no-install-recommends -y wget \
                                                  gcc \
                                                  git \
                                                  python3.6 python3-dev python-pip \
						  python-setuptools python-wheel \
                                                  python3-wheel python3-pip python3-venv \
						  build-essential \
                                                  libcurl4-gnutls-dev \
                                                  libffi-dev \
                                                  libssl-dev \
                                                  libxml2-dev \
                                                  libxslt1-dev \
						  libgnutls28-dev \
                                                  default-libmysqlclient-dev libsqlite3-dev \
						  curl \
    && rm -rf /var/lib/apt/lists/*

RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 10
RUN update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 10

# update pip to latest version (need to use /3.6/get-pip.py vk@240819)
RUN curl -s -L https://bootstrap.pypa.io/pip/3.6/get-pip.py | python
# fix tox<4 in order to reuse virtual environment during tests vk@240819
RUN pip install flake8 pylint pydocstyle pep8 bandit 'tox<4.0.0'
# otherwise issues with twine<=1.10.0 
RUN pip install twine==1.11.0 
RUN pip install -U wheel setuptools

# Standard SSH port
EXPOSE 22

# Default command
CMD ["/usr/sbin/sshd", "-D"]
