# README: Submission

## Solver track

- (Preperation) please do the followings:
  1. Develop your solver
     - (Check) your solver can accept [the input file format](https://core-challenge.github.io/2023/format/)?
	 - (Check) your solver can print [the output format](https://core-challenge.github.io/2023/format/) to standard out?
  2. Clone this repository [2023solver-submittion]() to 
  3. Create a solver executable on ubuntu 20.04 (as Docker container)
     - (Check) In your container, the following command returns appropriate output?
	 ```bash
	 /2023solver/run.sh /2023solver/example/hc-toyno-01.col /2023solver/example/hc-toyno-01_01.dat
	 /2023solver/run.sh /2023solver/example/hc-toyyes-01.col /2023solver/example/hc-toyyes-01_01.dat
	 ```
  4. Write your solver description. 
  5. Appropriately place i) your container archive and ii) your solver description into the your provate repository cloned from this repository [2023solver-submittion](). 
    - (Check) "Github action" will run everytime you push. Please check your action stateus to check whether your products are applopriately placed or not. 

- (Submission) please do the followings:
  1. please fill and submit [this Google Form]().
  2. submit your products as a private Github repository which contains
    - docker container archive
      - (Note) if your archive file exceeds 100MB (Github limit) then please contact us. We will let you know another link for uploading. 
	- solver description
  3. Add TakehideSoh and tom-tan as members of your private repository. 


<!-- - Please prepare the followings: -->
<!--   - [Example output](https://core-challenge.github.io/2022/#output-file-format) and "GNU time" command log. -->
<!--     - see detail [here](/solver/solver-track-instruction.md) -->
<!--   - solver executable on ubuntu 20.04 (as Docker container) -->
<!--     - see detail [here](/solver/solver-track-instruction.md) -->
<!--   - solver Description (in tex format) -->
<!--     - [example](/solver/doc/example.pdf). -->

<!-- - Template/example is given in [solver](/solver/). -->

<!-- - Submission should be done by the procedure described [here](https://core-challenge.github.io/2022/#for-solver-track). (clone this repository and add TakehideSoh in your private repository). -->

## About solver (as a docker container)

- Please submit your solver executable on Ubuntu 20.04 as a docker container archive (tar.gz archive).


### Dockerfile and final product

- The basic template is as follows.

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
# (2) clone the 2022solver directory to /
#------------------------------------------------
RUN git clone https://github.com/core-challenge/2022solver.git
```

- It will create an almost empty Ubuntu OS which has the `/2022solver/` directory.
- What you need to do is install your solver and rewrite `/2022solver/run.sh` so that `/2022solver/run.sh /2022solver/example/hc-toyno-01.col /2022solver/example/hc-toyno-01_01.dat` returns appropriate output.
- There are several ways to do that. The following is an instruction.

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
  - `docker export`

