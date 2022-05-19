ARG PYTHON_IMAGE_VERSION=3.8

FROM python:${PYTHON_IMAGE_VERSION}-slim

# Install required tools. Git is needed by Github Actions
# We might be able to remove gcc and python3-dev once we do not
# support 1.10 of Airflow
RUN apt-get update -qqy && apt-get install git gcc python3-dev -qqy 

# Upgrade pip
RUN pip install --upgrade pip

#
COPY constraints /tmp/constraints
# Install Airflow with GCP, Slack and OpsGenie along with test dependencies
ARG AIRFLOW_VERSION=2.2.3
RUN export VERSION_PYTHON="$(python --version | cut -d " " -f 2 | cut -d "." -f 1-2)" && \
    pip install --prefer-binary \
    apache-airflow[gcp,slack,opsgenie]==${AIRFLOW_VERSION} \
    pytest \
    flake8 \
    pytest-mock \
    PyHamcrest \
    --constraint "/tmp/constraints/${AIRFLOW_VERSION}/constraints-${VERSION_PYTHON}.txt"

# Clean up
RUN apt-get clean -qqy
RUN pip cache purge