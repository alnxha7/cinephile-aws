FROM python:3.11-slim

ENV PYTHONUNBUFFERED=1

# Set working directory
WORKDIR /cinephile_project

# Copy requirements
COPY cinephile_project/requirements.txt /tmp/requirements.txt
COPY cinephile_project/req_dev.txt /tmp/req_dev.txt

# Copy source code
COPY cinephile_project /cinephile_project

# Development flag
ARG DEV=false

# Create virtual env and install dependencies
RUN python -m venv /py && \
    /py/bin/pip install --upgrade pip && \
    /py/bin/pip install -r /tmp/requirements.txt && \
    if [ "$DEV" = "true" ]; then /py/bin/pip install -r /tmp/req_dev.txt ; fi && \
    rm -rf /tmp && \
    adduser --disabled-password --no-create-home django-user && \
    chown -R django-user:django-user /cinephile_project

# Set path
ENV PATH="/scripts:/py/bin:$PATH"

# Switch to user
USER django-user

# Expose port
EXPOSE 8000

# Default command
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
