# LaBiOS Minimal Linux Base

Dockerfile base for creating Docker projects by LaBiOS:

- Dockerfile base for the construction of Dugong Bioinformatics.
- Dockerfile base for the construction of SCIENV.

---

## Dockerfile Ubuntu:

- Compressed Size: 150 MB
- Packages default: curl git wget vim bzip2 sudo build-essential

```
FROM ubuntu:latest
MAINTAINER Fabiano Menegidio <fabiano.menegidio@biology.bio.br>

############### Environment config
ENV DEBIAN_FRONTEND noninteractive
ENV PASS dugong
ENV USER dugong
ENV HOME /headless

RUN apt-get update && apt-get install -y --allow-unauthenticated git curl wget vim bzip2 sudo build-essential \
    && apt-get clean && apt-get autoremove && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && echo "User: $USER Pass: $PASS" \
    && useradd -d $HOME --shell /bin/bash --user-group --groups adm,sudo $USER \
    && echo "$USER:$PASS" | chpasswd \
    && echo "$USER ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers \
    && mkdir -p $HOME/data \
    && mkdir -p $HOME/.ve \
    && mkdir -p $HOME/.workspace \
    && ln -s $HOME/.ve $HOME/data/.ve && ln -s $HOME/.workspace $HOME/data/.workspace \
    && chown -R $USER:$USER $HOME    
    
USER $USER

RUN echo "############### Welcome to Dugong Bioinformatics ###############" \
    && echo "############### USER: dugong - PASS: dugong ###############"
    
VOLUME ["$HOME/data"]
CMD ["/bin/bash"]
```
