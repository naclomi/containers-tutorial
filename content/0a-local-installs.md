
If you _don't_ see this information when running the above command, and you're on a recent Apple laptop, you may need to rebuild your docker container with a few extra options enabled. Run these two commands, with `[YOUR DOCKERHUB USERNAME]` replaced appropriately:

```bash
docker buildx create --name mybuilder --driver docker-container --bootstrap --use
docker buildx build -t [YOUR DOCKERHUB USERNAME]/my-textbook:latest -f Dockerfile --push --platform=linux/arm64,linux/amd64 .
```

At this point, you can restart the process from the `az container create` step and things should be working.



------------


If you're on Windows PowerShell (your command line starts with the letters "PS"), the command looks the same as above but with backticks at the end of the line instead of slashes:


```bash
az container create `
   --name my-cloud-textbook `
   --image naclomi/textbook-writer `
   --cpu 0.5 --memory 0.5 `
   --restart-policy Never --no-wait `
   --command-line "python3 src/main.py --pdf /output/text.pdf" `
   --azure-file-volume-account-name [STORAGE ACCOUNT NAME] `
   --azure-file-volume-account-key [STORAGE ACCOUNT KEY] `
   --azure-file-volume-share-name [FILE SHARE NAME] `
   --azure-file-volume-mount-path /output 
```
