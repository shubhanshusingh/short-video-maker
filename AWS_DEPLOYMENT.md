# AWS Deployment Guide for Short Video Maker

This guide walks through deploying the Short Video Maker service to AWS Lightsail using containers.

## Prerequisites

1. AWS CLI installed and configured
2. Docker installed
3. Node.js and pnpm installed
4. AWS account with appropriate permissions

## Step 1: Build the Docker Image

Build the image using the main-tiny.Dockerfile:

```bash
# Build the image with the correct platform
docker buildx build --platform linux/amd64 -t short-video-maker:tiny -f main-tiny.Dockerfile .
```

## Step 2: Set up ECR Repository

1. Create an ECR repository in ap-south-1:
```bash
aws ecr create-repository --repository-name short-video-maker --region ap-south-1
```

2. Login to ECR:
```bash
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 395750212628.dkr.ecr.ap-south-1.amazonaws.com
```

3. Tag the image:
```bash
docker tag short-video-maker:tiny 395750212628.dkr.ecr.ap-south-1.amazonaws.com/short-video-maker:latest
```

4. Push to ECR:
```bash
docker push 395750212628.dkr.ecr.ap-south-1.amazonaws.com/short-video-maker:latest
```

## Step 3: Deploy to Lightsail

1. Create a Lightsail container service (if not already created):
```bash
aws lightsail create-container-service \
  --service-name shortvideomaker \
  --power micro \
  --scale 1 \
  --region ap-south-1
```

2. Deploy the container with environment variables:
```bash
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

## Step 4: Monitor Deployment

1. Check deployment status:
```bash
aws lightsail get-container-services --service-name shortvideomaker --region ap-south-1
```

2. View container logs:
```bash
aws lightsail get-container-log --service-name shortvideomaker --container-name short-video-maker --region ap-south-1
```

## Accessing the Service

Once deployed, the service will be available at:
```
https://shortvideomaker.o34g8qgh0d75a.ap-south-1.cs.amazonlightsail.com/
```

The service exposes:
- MCP endpoint at `/mcp`
- API endpoint at `/api`
- UI server on port 3123

## Environment Variables

Required environment variables:
- `PEXELS_API_KEY`: Your Pexels API key
- `DATA_DIR_PATH`: Set to `/app/data` (default in Dockerfile)
- `DOCKER`: Set to `true` (default in Dockerfile)
- `WHISPER_MODEL`: Set to `tiny.en` (default in Dockerfile)
- `KOKORO_MODEL_PRECISION`: Set to `q4` (default in Dockerfile)
- `CONCURRENCY`: Number of chrome tabs for rendering (default: 1)
- `VIDEO_CACHE_SIZE_IN_BYTES`: Video cache size (default: 104857600)

## Troubleshooting

1. **Container fails to start**
   - Check container logs for errors
   - Verify environment variables are set correctly
   - Ensure the image is built for the correct platform (linux/amd64)

2. **Deployment takes too long**
   - Check the deployment status
   - Verify the container service is in the correct state
   - Check for any resource constraints

3. **Application not accessible**
   - Verify the public endpoint is configured correctly
   - Check if the container is running
   - Verify the port mapping is correct

## Cleanup

To delete the service:
```bash
aws lightsail delete-container-service --service-name shortvideomaker --region ap-south-1
```

To delete the ECR repository:
```bash
aws ecr delete-repository --repository-name short-video-maker --region ap-south-1 --force
``` 