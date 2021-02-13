# ecs-blue-grenn-deploy-sample
https://docs.aws.amazon.com/ja_jp/codepipeline/latest/userguide/tutorials-ecs-ecr-codedeploy.html

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

## 
