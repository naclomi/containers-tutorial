---
draft: false
title: "Part 2: Building containers"
aliases:
    /02-building-containers.html
---

Running docker containers in itself is a useful skill and you can do a lot with the pre-existing containers published on Docker Hub, but to fully customize the contents of a container you'll need to build your own images. In this section we'll walk through how.

## Building our first image

Images are defined through a **Dockerfile**, a little script that instructs Docker which files to copy into the container and what the entrypoint command is to be executed once the container is eventually run. Once we write our Dockerfile, we'll issue a terminal command that reads it in and actually builds the container image. Once *that's* done, we'll be able to run the container like we did in the previous section.

To get started, create a project folder for our work called `hello-world` with the following command:

```bash
mkdir hello-world
```

We'll be storing source code for our container image there. Next, move into it with this command:

```bash
cd hello-world
```

From here, create a new file called `Dockerfile` and open it for editing in VSCode. You can do that with the following command:

```bash 
code dockerfile
```

(In general, you can open existing files or create new ones with the commands of the form `code [filename]`)

Fill the `dockerfile` with the following contents:

```dockerfile
# My first Dockerfile
FROM python:3.6-stretch

ENTRYPOINT echo Hello World
```

Save the file with `File menu -> Save` or `Ctrl+S` and name it `Dockerfile`. At this point, you should see the text become **syntax highlighted** and the file appear in the Explorer sidebar on the left:

![first](../img/first.png)

Note that VSCode might have messed with the filename you entered, and done something like tack '.dockerfile' onto the end of the filename. If this is the case, (1) right-click the file you just saved and (2) select `Rename...`, then make sure the filename is just `Dockerfile`, no more or less:

![rename](../img/rename.png)

Now open a terminal (`Terminal menu -> New Terminal`) and run the command:

```bash
docker build -t [DOCKERHUB-USERNAME]/hello-world .
```

replacing `[DOCKERHUB-USERNAME]` with the username you used to sign up for Dockerhub. Make sure to include the period `.` at the end of the command :) !

The first time you run this command might take a few minutes to download all of the files required to build the container (it'll go faster next time!). Once it's done, it may output a variety of different success messages depending on your operating system. Ultimately, though, you shouldn't see the word 'fail' or 'no' anywhere in the output. One other way to check if the previous command succeeded is to run this subsequent "check" command:

```bash
echo $?
```

If the previous command succeeded, you'll see `0` or `True` outputted to the screen. If it failed, you'll see some other number, or the word `False`.

Once we run the above build command successfully, we can run our container either from the Docker sidebar or with the following terminal command:

```bash
docker run --rm [DOCKERHUB-USERNAME]/hello-world 
```

![hello_world_result_2](../img/hello_world_result_2.png)

You did it! You just built your first container image! So...what exactly did you just do? Let's start by walking through the dockerfile code you wrote.

## Dockerfile Syntax

A dockerfile consists of a series of commands, one per line. Comments start with a `#`, after which the rest of the line is ignored (just like Python). 

Our first command was **FROM**:

- `FROM [image-name]`
  This bases our new image off the **base image** specified by `[image-name]`, on top of which our changes get layered like new layers of paint on a painting. That name might refer to an image you already have on your computer, or possibly an image published on Docker Hub. Either way, all dockerfiles must start with a FROM command.

  In this case our base image was `python:3.6-stretch`, a container that already comes with Linux and Python 3.6 pre-installed. If we wanted to base an image off the latest version of Ubuntu Linux, it might look something like this: `FROM ubuntu:latest`.

Then, we issued an **ENTRYPOINT** command:

- `ENTRYPOINT [terminal-command]`
  This sets what command will be executed when the container image is actually run with `docker run`. Only the last ENTRYPOINT command in the Dockerfile will actually take effect, while the rest will be ignored.

  In this case, we issue the command `echo Hello World`, which prints "Hello World" to the terminal. Any command you normally type into the terminal could be put here, though keep in mind that command will only have access to what your Dockerfile copies into the container.

## The `build` command and image tags

To build a container, we run the `docker build` command which takes the form:

```bash
docker build -t [image-name] [project-directory]
```

The `-t [image-name]` flag specifies what we want to call, or **tag**, the image. We can actually specify as many tags as we like, by repeating the `-t [image-name]` flag with new names. By convention, names take the form:

```bash
[user-name]/[image-name]:[image-version]
```

where `[user-name]` is your Dockerhub username, `image-name` is what you'd like to call the image, and `[image-version]` is a version number like `1.0` or `latest` (the default version). If you leave out the `:[image-version]` section of the name, Docker will just assume the version is `latest`.

Finally, `[project-directory]` specifies the folder on your computer that contains the `Dockerfile` you would like to compile. In this case, since our terminal was already in the project directory, we just specified "the current location" with `.` .

## Build-time versus run-time

Let's add a new command to our Dockerfile:

```dockerfile
# My first Dockerfile
FROM python:3.6-stretch

# New command:
RUN echo Hello Galaxy

ENTRYPOINT echo Hello World

```

Save the file and re-build the docker container with this command:

```bash
docker build --no-cache --progress plain -t [DOCKERHUB-USERNAME]/hello-world . 
```
 
In general, you need to run `docker build` every time you modify the Dockerfile for your changes to actually take effect.

Once it's built, re-run the container with `docker run`.

Let's look at the output for all of this:

```bash
user@debian:~/hello-world$ docker build --no-cache --progress plain -t naclomi/hello-world .

#1 [internal] load build definition from Dockerfile
#1 sha256:703570748eba561ebf9bf7ed57fac1464db2eba2b487df882eccdae2488cfd20
#1 transferring dockerfile: 155B done
#1 DONE 0.0s

#2 [internal] load .dockerignore
#2 sha256:139fcf13ae9a8f8b9cf2374f6ad9a811f9d7a2c573f4cf870a18cd1cbdb711d9
#2 transferring context: 2B done
#2 DONE 0.0s

#3 [internal] load metadata for docker.io/library/python:3.6-stretch
#3 sha256:7cb3ce616859dfaaede09c8af55110806d0d186b2719d544c5463328c00bc1d9
#3 DONE 0.4s

#4 [1/2] FROM docker.io/library/python:3.6-stretch@sha256:654dd3f75f34c7075c118de1d7733613983140ab63bebdd2a5857e50b300924b
#4 sha256:a26bfb5fba3b2d8328b71b8c4c44aa04820538404263c11cf0070c63075488e6
#4 CACHED

#5 [2/2] RUN echo Hello Galaxy
#5 sha256:6fb84a7377ec14432e78276c23e72067ece30f8c215297d2f1e7a81d42a19954
#5 0.315 Hello Galaxy
#5 DONE 0.3s

#6 exporting to image
#6 sha256:e8c613e07b0b7ff33893b694f7759a10d42e180f2b4dc349fb57dc6b71dcab00
#6 exporting layers 0.0s done
#6 writing image sha256:36732b523b173113f1f57b81e2c46fd0fd6115b24061c929081f60ed76057c9f
#6 writing image sha256:36732b523b173113f1f57b81e2c46fd0fd6115b24061c929081f60ed76057c9f done
#6 naming to docker.io/naclomi/hello-world done
#6 DONE 0.0s

user@debian:~/hello-world$ docker run naclomi/hello-world 
Hello World
user@debian:~/hello-world$ 
```

![output_times](../img/output_times.png)

Gosh! Let's call attention to the place where our two `echo` messages showed up. The `echo` issued by the `RUN` command happened when we ran `docker build`, while the `echo` issued by `ENTRYPOINT` happened when we ran `docker run`.

This is a really important concept:

{{% aside %}}
🙀🙀🙀

**RUN commands happen at "build time", while the ENTRYPOINT command happens at "run time"**

🙀🙀🙀
{{% /aside %}}

In practice, this means the `RUN` commands only happen once when you build the container image, and then the `ENTRYPOINT` will happen every time any user out there in the wide world runs the image. This division of build time and run time is useful because it allows us to issue terminal commands that install software once at build time, and then never have to worry about them again.

Taking a step back, the best way to think about a Docker container might be as a fish tank: 

![fish_tank](../img/fish_tank.jpeg)*(Photo by [Sarah Brown](https://unsplash.com/@sweetpagesco?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on Unsplash)*

The Dockerfile is the instruction manual telling you how to assemble the tank. At build-time, you follow the instructions to pour rocks into the bottom of the tank and set up all of the tank's decorations. Then, at run time, you fill the tank with water and introduce the fish. The `RUN` commands specify how and where to put the tank decorations. The `ENTRYPOINT` command specifies what fish we will eventually put in the tank. The `docker run` terminal command actually pours the water and introduces the fish.

## Building a container with files

Let's start work on our textbook-writer image. Create a new folder on your computer somewhere other than within `hello-world`. Call the new folder `my-textbook`. Open the folder in VS Code, create a new empty file, and save it in the directory as `Dockerfile`. 

Then, download the source code and data for the textbook writer here and unzip it to the same location as the Dockerfile using these commands:

```bash
wget -P src https://raw.githubusercontent.com/naclomi/containers-tutorial/main/markov-example/src/main.py
wget -P data https://raw.githubusercontent.com/naclomi/containers-tutorial/main/markov-example/data/mechanics-of-materials.txt
```

Then, make an empty dockerfile with the command:

```bash
touch dockerfile
```

At this point, your project directory should have the following structure:

![textbook_tree](../img/textbook_tree.png)

Make sure your files are arranged this way before your proceed.

Next, let's put some code in our Dockerfile:

```dockerfile
FROM python:3.6-stretch

WORKDIR  /usr/src/textbook-writer
COPY src ./src
COPY data ./data

ENTRYPOINT python3 ./src/main.py
```

We've got a few new commands going on here. Let's take a closer look at them:

- **COPY**
  `COPY [source-file-or-folder][destination-file-or-folder]`
  At build-time, copies a file (or folder full of files), from your computer into the container.

  Note that the source path is relative to the directory structure  on your computer, while the destination path is relative to the directory structure in the container. We're going from one parallel universe to another.

  So, the way to read the first `COPY` command in our code is:
  "Take the `src` folder on my computer next to my `Dockerfile` (that's the one you just unzipped), and copy it into the container to the location `./src`".

  But the `.` in the destination path means "starting from the current directory". How do we know where in the container we're starting from? That's what the next command is for.

- **WORKDIR**
  `WORKDIR [container-path]`
  This sets the "working directory" that all future Dockerfile commands will operate from. It's like when we run `cd` from our terminal, but for Dockerfile commands instead of terminal commands. The path you specify refers to files inside the container, not on the rest of your computer.

So, our script sets the container's working directory to `/usr/src/textbook-writer`, copies the source and data directories we just unzipped into it, and then sets the entrypoint to be the python script we just `COPY`ed in.

Ohkay! Build the container, calling it `[DOCKERHUB-USERNAME]/my-textbook` (replacing the `[]` part with your username), and then run container!

And if all goes well, you should get.........

```bash
user@debian:~/my-textbook$ docker run --rm [DOCKERHUB-USERNAME]/my-textbook
Traceback (most recent call last):
  File "./src/main.py", line 4, in <module>
    import markovify
ModuleNotFoundError: No module named 'markovify'
user@debian:~/my-textbook$ 
```

This error. Like, very specifically this error (if you get another error, ask your TA for help).

What's going on here?

## Installing dependencies

A quick google for "markovify" turns up this:

{{% aside %}}
🔗 [https://pypi.org/project/markovify/](https://pypi.org/project/markovify/)
{{% /aside %}}

It looks like "markovify" is a Python library used for generating gibberish text. Our script **depends** on this library! Thank you for your beautiful work library authors!

We'll add a command to our Dockerfile to install this library at build time, so that nobody else has to worry about this software dependency ever again. Modify your Dockerfile to include the following RUN command:

```dockerfile
FROM python:3.6-stretch

# New line of code:
RUN pip3 install markovify

WORKDIR  /usr/src/textbook-writer
COPY src ./src
COPY data ./data

ENTRYPOINT python3 ./src/main.py
```

Recall that `RUN` executes a terminal command inside of the container at build time. The command in this case, `pip3 install markovify`, asks the Python package manager ("pip") to download and install the `markovify` library for us (all inside the container). That command is the same kind of thing you could run from your terminal without docker.

Now rebuild the image and rerun the container. You should get some quality garbage output:

![our_own_output](../img/our_own_output.png)

Hooray! But what if we want only one sentence of output? We can just pass it as a command line argument, right?

```bash
docker run --rm [DOCKERHUB-USERNAME]/my-textbook --sentences 1
```

![too_many_cooks](../img/too_many_cooks.png)

......hmm. That looks like.....it didn't work.

## Taking command-line arguments

To get our container to accept command line arguments, we're going to have to make a small change to the way we use the `ENTRYPOINT` command. It turns out, the command has two forms. Here's the one we used:

```dockerfile
ENTRYPOINT python3 ./src/main.py
```

This is called **shell form**, because it's how we enter commands in our own terminal shell. Here is the other form:

```dockerfile
ENTRYPOINT ["python3", "./src/main.py"]
```

This is called **exec form**. Instead of typing the command out normally, we surround it with square brackets (`[]`) and surround each flag with double-quotes `""`, separating them with commas. Basically, take your shell form, separate the text into words based on the spaces, surround each word with quotes, and put them in `[]`s.

Docker containers will only pass command line arguments to the entrypoint if it is specified in exec form. So let's update our dockerfile:

```dockerfile
FROM python:3.6-stretch

RUN pip3 install markovify

WORKDIR  /usr/src/textbook-writer
COPY src ./src
COPY data ./data

ENTRYPOINT ["python3", "./src/main.py"]
```

Rebuild and rerun:

![good_number_of_cooks](../img/good_number_of_cooks.png)

Muuuuuch better. Our command line flag asking for only one sentence now successfully makes it to the python script.

## Default arguments

We can actually specify default flags to be passed to the entrypoint, that disappear if the user specifies their own when they run the container. To do that, we use the **CMD** command:


`CMD ["flag1", "flag2", ...]`

At run-time, the `CMD` command will glue whatever flags you specify in its list to the end of the list of flags used by `ENTRYPOINT` unless the user specifies their own command-line flags, in which case it will do nothing. Similar to `ENTRYPOINT`, only the last `CMD` in your Dockerfile will actually take effect. And similar to user-supplied command line flags, `ENTRYPOINT` has to be in exec form for `CMD` to do anything.

So! If we wanted to by default generate 4 sentences of text, unless the user specified otherwise, we could write a Dockerfile like this:

```dockerfile
FROM python:3.6-stretch

RUN pip3 install markovify

WORKDIR  /usr/src/textbook-writer
COPY src ./src
COPY data ./data

ENTRYPOINT ["python3", "./src/main.py"]
CMD ["--sentences", "4"]
```

![cmd_ex](../img/cmd_ex.png)

Not a necessary feature by any means, but potentially useful.

## Stretch challenge #1

Replace the source text that the generator script trains off of with your own content, to create a text generator for something other than a matsci textbook :)

## Stretch challenge #2

If you try generating a PDF as you did in the "Running containers" portion of this tutorial, you'll find that your container still doesn't fully work. It's missing the programs our script uses to save PDF files: `ghostscript` and `enscript`.

To install them, you'll need to add a command to the right place in your Dockerfile that uses the Linux package manager `apt-get` . See if you can get it working.

*🤫 Hint: the first thing you'll have to do is run the command `apt-get update`* 

