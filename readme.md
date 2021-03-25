# Containerization Tutorial

## What are we doing here?

#### Tutorial Goals

In this tutorial series we are going to learn how to:

- Download and run applications packaged within Docker containers
- Build our own Docker containers from scratch
- Deploy our Docker containers to Azure

#### Requirements and expectations

TODO:

VSCode, Docker, Python, Azure account

## Background

#### Containers

**Containers** are a technology that let you easily reproduce the environment your code runs in on other computers and servers. This environment includes all the software, libraries, and hand-tweaked configuration you need to run your experiments and reproduce their results.

You could think of containers as very **light-weight** virtual machines: they run faster and take up less disk space than traditional VMs, while still giving the appearance of an isolated computer to the applications running within.

That said, they are more often used to **run code** than as an **interactive desktop computer** -- you won't usually open a terminal within a container or run graphical applications from them. Rather, once you develop your code, you will **package it** into a **container image** which can be **run reproducibly**.

#### Docker

**Docker** containers are one implementation of the container concept, which we will be working with for the remainder of this tutorial. Docker container images can be **published** for free on **Docker Hub**, which hosts Docker images the same way Github hosts code and Google Docs hosts word documents.

The most common way of interacting with Docker on your computer is with the Docker Command Line Interface (**Docker CLI**), but there is also a wonderful **VS Code Docker plugin** that lets us do many of the same operations through VS Code's GUI. We will be using that as well.

## Installing Docker

Installing Docker can be an unfortunately involved process depending on what operating system 

- https://carpentries-incubator.github.io/docker-introduction/setup.html

After you have Docker installed, sign up for an account on Docker Hub:

https://hub.docker.com/


## Notes and References

- Some content adapted from The Carpentries' [Docker lesson](https://carpentries-incubator.github.io/docker-introduction/), retrieved on March 10th, 2021

- Markov text generator trained on portions of *Mechanics of Materials*, Wiley ISBN 0-471-59399-0, retrieved from https://ocw.mit.edu/courses/materials-science-and-engineering/3-11-mechanics-of-materials-fall-1999/modules/ on March 5th, 2021

- For a full reference of `Dockerfile` commands, refer to:
  https://docs.docker.com/engine/reference/builder/

- For a full reference of the `az container` set of CLI commands, refer to:
  https://docs.microsoft.com/en-us/cli/azure/container?view=azure-cli-latest

## TODO

- Fit in somewhere: If an image with the name you specify in the build command already exists on your computer, rather than getting overwritten or deleted it continues to exist on your computer with a gibberish name:

- Describe how to use `\` to split a command across multiple lines in a Dockerfile

- Describe deploying account passwords using environment variables

- Teach layering and ordering concepts

  