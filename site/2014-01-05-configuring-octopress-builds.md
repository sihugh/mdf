---
layout: post
title: Configuring Windows to enable Octopress builds
---

- Install Chocolatey from [https://chocolatey.org/](https://chocolatey.org/) (Not strictly necessary, but it is a handy tool)

- Install Git 

```
    cinst git
```

- Install Ruby 1.9.3 from [http://rubyinstaller.org/downloads/](http://rubyinstaller.org/downloads/ "Ruby Installer")

- Install Ruby Devkit 

```
cinst ruby.devkit
```

- Clone Octopress locally ("git clone https://github.com/imathis/octopress ./octopress" from command line)

- To ensure that all the Gem dependencies are correctly installed:

```
    cd octopress
    
    gem install bundler
    
    bundle install
```
    
- Put content in the source/_posts folder

- To build the site:

```
    rake generate
```



