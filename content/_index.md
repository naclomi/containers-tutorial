---
draft: false
title: "Containerization Tutorial"
---

# Containerization Tutorial

## What are we doing here?

### Tutorial Goals

In this tutorial series we are going to learn how to:

- Download and run applications packaged within Docker containers
- Build our own Docker containers from scratch
- Deploy our Docker containers to Azure

### Requirements and expectations

This tutorial series makes extensive use of VS Code, Docker, VS Code's Azure and Docker extensions, and the Azure CLI. It assumes you have these installed on your system before proceeding. On Windows, it also assumes you have WSL2 installed, which is a requirement for Docker. For instructions on how to install each, please see:

- [WSL2](https://learn.microsoft.com/en-us/windows/wsl/install) (only needed on Windows)
- [VS Code](https://code.visualstudio.com/Download)
- [VS Code Azure plugin set](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) (includes Docker plugin)
- [Docker](https://docs.docker.com/desktop/)
- [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)

The tutorial content makes light use of shell scripting and Python. It does not expect you to write any such scripts on your own, but it will help to have a passing familiarity with them.

Parts 1 and 2 all run locally but will require a free Docker Hub account, which you can register for [here](https://hub.docker.com/signup)


Part 3 walks through cloud deployment, for which you will need Microsoft Azure login credentials.


## Background

### Containers

**Containers** are a technology that let you easily reproduce the environment your code runs in on other computers and servers. This environment includes all the software, libraries, and hand-tweaked configuration you need to run your experiments and reproduce their results.

You could think of containers as very **light-weight** virtual machines: they run faster and take up less disk space than traditional VMs, while still giving the appearance of an isolated computer to the applications running within.

That said, they are more often used to **run code** than as an **interactive desktop computer** -- you won't usually open a terminal within a container or run graphical applications from them. Rather, once you develop your code, you will **package it** into a **container image** which can be **run reproducibly**.

### Docker

**Docker** containers are one implementation of the container concept, which we will be working with for the remainder of this tutorial. Docker container images can be **published** for free on **Docker Hub**, which hosts Docker images the same way Github hosts code and Google Docs hosts word documents.

The most common way of interacting with Docker on your computer is with the Docker Command Line Interface (**Docker CLI**), but there is also a wonderful **VS Code Docker plugin** that lets us do many of the same operations through VS Code's GUI. We will be using that as well.

## Installing Docker

Installing Docker can be an unfortunately involved process depending on what operating system you are using. For a general-purpose set-up guide, see here:

[https://carpentries-incubator.github.io/docker-introduction/setup.html](https://carpentries-incubator.github.io/docker-introduction/setup.html)

After you have Docker installed, sign up for an account on Docker Hub:

[https://hub.docker.com/](https://hub.docker.com/)

## Tutorial Modules

### [Part 1: Running Containers](01-running-containers)

### [Part 2: Building Containers](02-building-containers)

### [Part 3: Deploying Containers](03-deploying-containers)

## Notes and References

- Some content adapted from The Carpentries' [Docker lesson](https://carpentries-incubator.github.io/docker-introduction/), retrieved on March 10th, 2021

- Markov text generator trained on portions of *Mechanics of Materials*, Wiley ISBN 0-471-59399-0, retrieved from https://ocw.mit.edu/courses/materials-science-and-engineering/3-11-mechanics-of-materials-fall-1999/modules/ on March 5th, 2021

- For a full reference of `Dockerfile` commands, refer to:
  [https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/)

- For a full reference of the `az container` set of CLI commands, refer to:
  [https://docs.microsoft.com/en-us/cli/azure/container?view=azure-cli-latest](https://docs.microsoft.com/en-us/cli/azure/container?view=azure-cli-latest)

## TODOs

- Fit in somewhere: If an image with the name you specify in the build command already exists on your computer, rather than getting overwritten or deleted it continues to exist on your computer with a gibberish name:

- Describe how to use `\` to split a command across multiple lines in a Dockerfile

- Describe deploying account passwords using environment variables

- Teach layering and ordering concepts

- Fit in somewhere: an exercise trying to access host-side files from inside a container shell
