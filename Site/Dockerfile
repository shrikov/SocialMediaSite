FROM python:3.9-slim

WORKDIR /app

RUN apt-get update && apt-get install -y \
    gcc \
    postgresql-client \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# Copy and install requirements
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
RUN pip install gunicorn psycopg2-binary whitenoise

# Copy the entire socials directory contents directly to /app
COPY socials/ /app/

# Create directories for static and media
RUN mkdir -p /app/staticfiles /app/media

# Create startup script
RUN echo '#!/bin/bash\n\
cd /app\n\
python manage.py makemigrations --noinput\n\
python manage.py migrate --noinput\n\
python manage.py collectstatic --noinput\n\
gunicorn --bind 0.0.0.0:8000 socials.wsgi:application' > /app/start.sh

RUN chmod +x /app/start.sh

EXPOSE 8000

CMD ["/app/start.sh"]
