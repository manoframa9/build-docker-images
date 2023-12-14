FROM python:3.8-slim-bookworm

RUN apt-get update --fix-missing && \
    apt-get install -y build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libsqlite3-dev libreadline-dev libffi-dev curl libbz2-dev unixodbc-dev

WORKDIR /usr/src/app
COPY ./requirements.txt .
# https://learn.microsoft.com/en-us/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server?view=sql-server-ver15&tabs=alpine18-install%2Cdebian17-install%2Cdebian8-install%2Credhat7-13-install%2Crhel7-offline
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
    curl https://packages.microsoft.com/config/debian/11/prod.list > /etc/apt/sources.list.d/mssql-release.list && \
    apt-get update --fix-missing && \
    ACCEPT_EULA=Y apt-get install -y msodbcsql17 && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/* && \
    pip install wheel && \
    pip install -r requirements.txt

# Fix Prisma issues
RUN rm -rf /usr/local/lib/python3.8/site-packages/gevent/tests/*.pem
RUN rm -rf /usr/local/lib/python3.8/site-packages/gevent/tests/*.key
RUN rm -rf /usr/local/lib/python3.8/site-packages/ndg/httpsclient/test/pki/localhost.key
RUN rm -rf /usr/local/lib/python3.8/site-packages/tornado/test/test.key

# Harden Image
COPY ./hardening.sh .
RUN chmod +x hardening.sh && \
   sh hardening.sh && \
   rm hardening.sh

# Create a non-root user (Hardening script doesn't create an appuser for Debian)
RUN useradd -ms /bin/bash appuser
USER appuser
