# Build the image
docker build -t flask-hello-world:latest .

# Run locally
docker run -p 5000:5000 flask-hello-world:latest

# Test the endpoints
curl http://localhost:5000/
curl http://localhost:5000/health
curl http://localhost:5000/ready
