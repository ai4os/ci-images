FROM python:3.12
LABEL org.opencontainers.image.authors='Pablo Orviz <orviz@ifca.unican.es>, Valentin Kozlov <valentin.kozlov@kit.edu>, Alvaro López <aloga@ifca.unican.es>'

RUN apt-get update \
    && apt-get install --no-install-recommends -y wget \
                                                  git \
                                                  build-essential \
                                                  curl \
    && rm -rf /var/lib/apt/lists/*

# update pip to latest version
RUN curl -s -L https://bootstrap.pypa.io/get-pip.py | python

# Install needed Python packages
# fix tox<4 in order to reuse virtual environment during tests vk@240819
RUN pip install --no-cache-dir 'tox<4.0.0' flake8 pylint pydocstyle pep8 bandit twine wheel setuptools poetry

RUN pip install --no-cache-dir ai4-metadata oscar-python liboidcagent

# Default command, keep the container working
CMD ["/usr/bin/tail", "-f", "/dev/null"]
