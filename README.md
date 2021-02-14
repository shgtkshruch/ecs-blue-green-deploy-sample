# ecs-blue-green-deploy-sample
https://docs.aws.amazon.com/ja_jp/codepipeline/latest/userguide/tutorials-ecs-ecr-codedeploy.html

1. Cerate Docker image and push to ECR
1. Create ECS task definition and AppSpec source files
1. Create ALB and taget groups
1. Create CodeDeploy application and deployment group
1. Create deployment pipeline on GitHub Actions
1. Update `static-html-directory/index.html` and push Gitub

## Create Nginx repository in ECR

```sh
# Pull Nginx image
$ docker pull nginx

# Create ECR repository
$ aws ecr create-repository --repository-name nginx

# Add a tag to Nginx image
$ docker tag nginx:latest 353381651656.dkr.ecr.ap-northeast-1.amazonaws.com/nginx:latest

# Login ECR
$ aws ecr get-login-password --region ap-northeast-1 | \
  docker login --username AWS --password-stdin 353381651656.dkr.ecr.ap-northeast-1.amazonaws.com/nginx

# Push Nginx image to ECR
$ docker push 353381651656.dkr.ecr.ap-northeast-1.amazonaws.com/nginx:latest
```

## Register task definition

```sh
# Register task definition
$ aws ecs register-task-definition --cli-input-json file://taskdef.json
```

## Creat ECS service
```sh
# Create ECS service
$ aws ecs create-service --service-name my-service --cli-input-json file://create-service.json

# Confirm
$ aws ecs describe-services --cluster ecs-blue-green-deploy --services my-service
```

## Update Nginx

https://hub.docker.com/_/nginx

```sh
$ docker build -t 353381651656.dkr.ecr.ap-northeast-1.amazonaws.com/nginx:latest .
$ docker run -d -p 8080:80 353381651656.dkr.ecr.ap-northeast-1.amazonaws.com/nginx:latest
```

Open `localhost:8080` in user browser.
