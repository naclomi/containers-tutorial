
If you _don't_ see this information when running the above command, and you're on a recent Apple laptop, you may need to rebuild your docker container with a few extra options enabled. Run these two commands, with `[YOUR DOCKERHUB USERNAME]` replaced appropriately:

```bash
docker buildx create --name mybuilder --driver docker-container --bootstrap --use
docker buildx build -t [YOUR DOCKERHUB USERNAME]/my-textbook:latest -f Dockerfile --push --platform=linux/arm64,linux/amd64 .
```

At this point, you can restart the process from the `az container create` step and things should be working.

