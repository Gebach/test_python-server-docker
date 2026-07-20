FROM python:3.9-alpine

RUN addgroup -r appgroup && adduser -r appuser -G appgroup

WORKDIR /app

COPY app.py .

RUN chown -R appuser:appgroup /app

USER appuser

EXPOSE 8080

CMD ["python", "app.py"]
