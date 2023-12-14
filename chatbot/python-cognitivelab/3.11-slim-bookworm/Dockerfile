FROM python:3.10-slim-bookworm

WORKDIR /app
COPY requirements.txt /app/
RUN pip install --upgrade pip && \
    pip install wheel setuptools --upgrade && \
    pip install --no-cache-dir -r requirements.txt

# Harden Image
# For Debian, need to "not" use sh when executing the script so that bash is used.
# Otherwise, there will be a "Syntax error: redirection unexpected" error
# Also need to add execute permission to do this.
COPY ./hardening.sh .
RUN chmod +x hardening.sh && \
    ./hardening.sh && \
    rm hardening.sh

# Change to a non-root user
USER appuser
