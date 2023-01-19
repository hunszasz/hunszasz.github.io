---
layout: post
title: Jekyll GitHub oldal létrehozása
date: 2023-01-19 11:25 +0100
categories: [Működés, Hasznos]
---
# Cél
GitHub personal page létrehozása, ahol tartalommegosztást is véghez lehet vinni.

# Választott eszköz
Jekyll

# Lépések

## GitHub repo inicializáció

A GitHub-on hozz létre egy **public** repo, a következő névkonvencióval: <username>.github.io
* Repository->settings
* "Code and automation" kattints a **Pages**-re
* "Build and deployment" Source alatt válaszd ki a **Deploy from branch**
* "Build and deployment" Branch alatt válaszd a **None** helyett a **main**-t vagy amit használni szeretnél
* **Save** mentés

## Jekyll telepítés (Fedora 37)

```shell
sudo dnf install ruby ruby-devel openssl-devel redhat-rpm-config @development-tools

echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

gem install jekyll bundler
```

## Jekyll init

```shell
#init github **public** repo, with name convention <username>.github.io
cd hunszasz.github.io

jekyll new --skip-bundle .
# Creates a Jekyll site in the current directory
```

Gemfile kiegészítése 
```shell
gem install jekyll-compose
```
majd a könyvtárban
```shell
bundle
```

## Új bejegyzés létrehozása
```shell
bundle exec jekyll post "Miről szól a blog?"
```

## Kipróbálás
```shell
bundle exec jekyll serve
```
böngészőben a http://localhost:4000 megnyitása.
Ezek után már csak fel kell tölteni a forrását. Javaslom az alábbi .gitignore fájlt használni:
```
_site
.sass-cache
.jekyll-cache
.jekyll-metadata
vendor
.bundle
Gemfile.lock
```

# Hasznos linkek
* [GitHub jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)
* [jekyll test](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll)
* [jekyll best practices](https://climbtheladder.com/10-jekyll-website-best-practices/)
* [jekyll install](https://jekyllrb.com/docs/installation/)