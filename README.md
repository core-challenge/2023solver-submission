# README: Submission

## ToDo before Submission

  1. Develop your solver
     - (Check) your solver can accept [the input file format](https://core-challenge.github.io/2023/format/)?
     - (Check) your solver can print [the output format](https://core-challenge.github.io/2023/format/) to standard out?
  2. Clone this repository [2023solver-submission](https://github.com/core-challenge/2023solver-submission) and edit it as your private repository. 
  3. Create a solver executable on ubuntu 20.04 as a Dockerfile.
  4. Write your solver description. 
  5. The final state of your Github private repository is:
     - `container/Dockerfile` is edited and `docker build ./` will build your solver docker image.
     - Using your docker image, `docker run --rm -it /solver-exe/run.sh input.col input.dat` will print result.
     - `description/main.tex` can be compiled using `pdflatex`.

## Dockerfile

- You can find the official reference of Dockerfile [here](https://docs.docker.com/engine/reference/builder/).
- What you need to do is
  - prepare your solver and put all necessary files into YOUR-SOLVER-MATERIAL-IN-CONTAINER-DIR (any name you want).
  - copy YOUR-SOLVER-MATERIAL-IN-CONTAINER-DIR into the `container` directory.
  - edit `container/run.sh` so that your solver can run.
  - edit the Dockerfile below so that your solver can run.

``` bash
FROM ubuntu:20.04

#------------------------------------------------
# (1) install fundamental commands
#------------------------------------------------
RUN \
    apt update && \
    apt -y upgrade && \
    apt install -y curl git man unzip vim wget sudo

#   Hint: you may want to additionally install the followings. 
#   apt install -y build-essential
#   apt install -y software-properties-common

#------------------------------------------------
# (2) install your solver
#------------------------------------------------
RUN \
    mkdir solver-exe && \
    cd solver-exe && \
    COPY YOUR-SOLVER-MATERIAL-IN-CONTAINER-DIR . && \
    ## .. put your solver install commands ..

#------------------------------------------------
# (3) construct your solver directory
# before copying please edit run.sh so that the command
#    ./run.sh input.col input.dat 
# will run your solver and print results appropriately
#------------------------------------------------
RUN \
    cd solver-exe && \
    COPY run.sh .
```

## Check List before Submission

- [ ] your solver can accept [the input file format](https://core-challenge.github.io/2023/format/)?
- [ ] your solver can print [the output format](https://core-challenge.github.io/2023/format/) to standard out?
- [ ] In your container, does the following command returns the appropriate output?
- [ ] Using your docker image, `docker run --rm -it /solver-exe/run.sh input.col input.dat` will print appropriate results?
- [ ] Are "Github action" status all green?

## ToDo at the Submission

  1. please fill in and send your information through [this Google form](https://forms.gle/CGYfrksJASwGUpWYA).
  2. submit your products as a private Github repository which contains
    - a Docker file
    - a solver description
  3. Add TakehideSoh and tom-tan as members of your private repository. 

<!-- 
### Instruction

- At first, edit the above Dockerfile as you like (cloning 2022solver is mandatory).

- Then, build your docker image by the following command. Note that `mysolver` can be any "image name" and `v01` can be any "tag name".

```bash
docker build -t mysolver:v01 .
```

- It may take minutes. 
- If the command is successfully finished, `[+] Building xxx.xs (y/y) FINISHED` is displayed. 

`$ docker image ls` shows the image you just created. 

```bash
$ docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
mysolver     v01       af8aaca552c0   3 minutes ago   277MB
```

- Next, let's launch your "container" from the image. 

```bash
$ docker run -it --name mysolver-container mysolver:v01 bash
root@13765b36541e:/# 
root@13765b36541e:/# ls 2022solver/
README.md  example  run.sh
```

- `root@13765b36541e:/# ` is the prompt in the running container. 
- Then, let's detach from your container by `ctrl-pq` (holding down `ctrl`, and type `p` and type `q`) which will keep your container alive (do not use `exit` or `ctrl-d` which will stop your container). 
- You get back to the prompt of your local machine. Then, `docker ps` will show your container running. 

```bash
$ docker ps -a
CONTAINER ID   IMAGE          COMMAND   CREATED          STATUS          PORTS     NAMES
13765b36541e   mysolver:v01   "bash"    13 minutes ago   Up 13 minutes             mysolver-container
```

- If you want to copy some file/directory on your local machine into the running container, please use `docker cp` as follows:

```bash
docker cp solver-track-instruction.md mysolver-container:/2022solver/
```

- In this example, a file `solver-track-instruction.md` is copied to `/2022solver/` of `mysolver-container`. 
- To re-enter a running container, use `docker exec` as follows. You can find the file `solver-track-instruction.md` copied from your local machine. 

```bash
$ docker exec -it mysolver-container bash
root@13765b36541e:/# ls 2022solver/
README.md  example  run.sh  solver-track-instruction.md
```

- By using the commands explained above, please install your solver and rewrite `run.sh` so that the command `/2022solver/run.sh /2022solver/example/hc-toyno-01.col /2022solver/example/hc-toyno-01_01.dat` returns appropriate output. 

- After completing your installation, the final task is `docker export`.
- If you are in the running container, please detach by `ctrl-pq`.
- Check your container exists `docker ps -a`. 

```bash
$ docker ps -a
CONTAINER ID   IMAGE          COMMAND   CREATED          STATUS          PORTS     NAMES
13765b36541e   mysolver:v01   "bash"    39 minutes ago   Up 39 minutes             mysolver-container
``` 

- Then, execure the following command, which will create your solver archive. 

```bash
$ docker export mysolver-container | gzip -c > mysolver-container.tar.gz
```

- Please add `mysolver-container.tar.gz` to your repository at your submission. 

### Docker command reference

- The official reference of Docker CLI is [here](https://docs.docker.com/engine/reference/run/).
- The followings are frequently used commands.
  - `docker build`
  - `docker image ls`
  - `docker ps -a`
  - `docker cp`
  - `docker exec`
  - `docker export` -->

