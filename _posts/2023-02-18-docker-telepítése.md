---
layout: post
title: Docker telepítése
date: 2023-02-18 17:03 +0100
categories: [Hasznos]
---

# Cél
Docker konténerek futtatása előtti szükséges lépés, Docker telepítése.

# Telepítés

```shell
sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

# Indítás

```shell
sudo systemctl start docker
```

# Boot során engedélyezés

```shell
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

# Non-root használat

```shell
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker

mkdir $HOME/.docker
sudo chmod g+rwx "$HOME/.docker" -R
```

# Teszt 
```shell
docker run hello-world
```

# Hasznos linkek
* [Docker fedora install](https://docs.docker.com/engine/install/fedora/)