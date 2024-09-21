FROM python:3.12
WORKDIR /workspace
COPY requirements.lock .
RUN pip install --no-cache-dir -r requirements.lock
EXPOSE 3001
CMD ["python", "manage.py", "runserver", "0.0.0.0:3001"]