---
draft: false
title: "Appendix: Using Docker on your personal computer"
aliases:
    /0a-local-installs.html
---

The containerization examples in this tutorial are written assuming they are being run in a Linux environment, but Docker can be installed on your personal computer and be invoked with almost exactly the same commands. This appendix details the few differences you should be aware of.

# Installing Docker

Installing Docker can be an unfortunately involved process depending on what operating system you are using. For a general-purpose set-up guide, see here:

{{% aside %}}
🔗[https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)
{{% /aside %}}

If you're on Windows, you may have to install WSL2 ("Windows Subsystem for Linux 2") first. There are instructions on doing so here:

{{% aside %}}
🔗[https://learn.microsoft.com/en-us/windows/wsl/install
](https://learn.microsoft.com/en-us/windows/wsl/install
)
{{% /aside %}}


# Apple/MacOS concerns 

## Building cloud-compatible images on Apple silicon

If you're on an computer running Apple silicon, you need to run special commands to build container images that can be run in the Azure cloud.

First, check if you're on Apple silicon using this guide:

{{% aside %}}
🔗[https://support.apple.com/en-us/116943](https://support.apple.com/en-us/116943)
{{% /aside %}}

If so, and your containers don't seem to be working when you submit them to the cloud with `az container create`, you may need to rebuild the container image with a few extra options enabled.

Run these two commands, with `[YOUR DOCKERHUB USERNAME]` and `[YOUR IMAGE NAME]` replaced appropriately:

```bash
docker buildx create --name mybuilder --driver docker-container --bootstrap --use
docker buildx build -t [YOUR DOCKERHUB USERNAME]/[YOUR IMAGE NAME]:latest -f Dockerfile --push --platform=linux/arm64,linux/amd64 .
```

These commands builds and publishes the image for the Intel processors that the Azure cloud uses (in addition to the image built for Apple processors, that the normal `docker build` command produces). At this point, you should restart the process from the `az container create` step and things should be working.

If `az container create` _still fails_, giving you an error with the text `InaccessibleImage`, you can just use the reference image built for this tutorial titled `naclomi/textbook-writer-cloudref` (instead of the image you created). This error is related to the added complexity of building Docker containers compatible with Apple computers, and can be circumvented with some further tricks that are unfortunately outside the scope of what we have time for here.

# Windows concerns 

## Figuring out what shell you're in

When you open terminals on Windows, they can be in a number of different environments (called "shells"). Here is how to identify which you're in:


- Try the command `uname -a`. If it succeeds, you're in a **UNIX-like shell**, probably **bash**. The tutorial commands should work as-is.
- Try the command `Get-Host`. if it succeeds, you're in **PowerShell**. See the notes below.
- Try the command `ver`. If it succeeds and prints a line containing `Microsoft Windows`, you're using the **Windows Command prompt**. This shell is too rudimentary to run the tutorial commands. You can turn it into a bash shell by running the command `wsl`, or PowerShell by running the command `powershell`

## Multi-line commands in PowerShell

In the UNIX-style commands the tutorial uses, some terminal commands are continued across multiple lines by ending each line with a backslash `\`. In PowerShell, these `\`s have to be replaced with backticks (`` ` ``). As an example, the multi-line `az container create` command from the cloud part of this tutorial would look like this:

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
   --azure-file-volume-mount-path output 
```
