# Deployment Commands Quick Reference

## Build and Push Image

```bash
# Build image
docker buildx build --platform linux/amd64 -t short-video-maker:tiny -f main-tiny.Dockerfile .

# Login to ECR
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 395750212628.dkr.ecr.ap-south-1.amazonaws.com

# Tag image
docker tag short-video-maker:tiny 395750212628.dkr.ecr.ap-south-1.amazonaws.com/short-video-maker:latest

# Push to ECR
docker push 395750212628.dkr.ecr.ap-south-1.amazonaws.com/short-video-maker:latest
```

## Deploy to Lightsail

```bash
# Deploy with environment variables
aws lightsail create-container-service-deployment \
  --service-name shortvideomaker \
  --containers '{
    "short-video-maker": {
      "image": "395750212628.dkr.ecr.ap-south-1.amazonaws.com/short-video-maker:latest",
      "ports": {
        "3123": "HTTP"
      },
      "environment": {
        "PEXELS_API_KEY": "YOUR_PEXELS_API_KEY"
      }
    }
  }' \
  --public-endpoint '{
    "containerName": "short-video-maker",
    "containerPort": 3123
  }' \
  --region ap-south-1
```

## Monitor Deployment

```bash
# Check status
aws lightsail get-container-services --service-name shortvideomaker --region ap-south-1

# View logs
aws lightsail get-container-log --service-name shortvideomaker --container-name short-video-maker --region ap-south-1
```

## Cleanup

```bash
# Delete service
aws lightsail delete-container-service --service-name shortvideomaker --region ap-south-1

# Delete ECR repository
aws ecr delete-repository --repository-name short-video-maker --region ap-south-1 --force
``` 