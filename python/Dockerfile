FROM ai4oshub/ci-images:base
LABEL org.opencontainers.image.authors='Pablo Orviz <orviz@ifca.unican.es>, Valentin Kozlov <valentin.kozlov@kit.edu>'

RUN apt-get update \
    && apt-get install --no-install-recommends -y wget \
                                                  gcc \
                                                  git \
                                                  python python-dev python-pip \
						  python3-dev \
						  python-setuptools python-wheel \
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

# We need:
#
# tox>=2.7.0 to avoid errors when not including 'commands' in [testenv] section
#	https://github.com/tox-dev/tox/issues/454
# virtualenv>=16.0.0 to avoid failures when executing tox within an absolute path
#		     containing whitespaces
#	https://github.com/tox-dev/tox/issues/121
# fix tox<4 in order to reuse virtual environment during tests vk@240819

RUN curl -s -L https://bootstrap.pypa.io/get-pip.py | python
RUN pip install flake8 pylint pydocstyle pep8 bandit 'tox>=2.7.0,<4.0.0'
# otherwise issues with twine<=1.10.0
RUN pip install twine==1.11.0
RUN pip install -U wheel setuptools

# Add tox binary to $PATH
ENV PATH="${PATH}:/usr/local/bin"

# Standard SSH port
EXPOSE 22

# Default command
CMD ["/usr/sbin/sshd", "-D"]
