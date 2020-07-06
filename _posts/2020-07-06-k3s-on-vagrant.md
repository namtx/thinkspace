---
layout: post
title: "k3s on vagrant"
description: "How to provision a simple k3s cluster by Vagrant"
comments: true
keywords: "k3s"
---

# Installation
[Install Vagrant](https://www.vagrantup.com/intro/getting-started/install)

# Commands

```bash
vagrant up

vagrant ssh

vagrant halt
```

# K3s on Vagrant

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"

  config.vm.define "ubuntu-k3s" do |archlinux_k3s|
    config.vm.network "forwarded_port", guest: 8001, host: 8001, auto_correct: true
    config.vm.network "forwarded_port", guest: 9110, host: 9110, auto_correct: true

    config.vm.provider "virtualbox" do |vb|
      vb.name = "Ubuntu k3s"
      vb.memory = "1024"
      vb.cpus = "1"
    end

    args = []
    config.vm.provision "shell", path: "scripts/install_k3s.sh", args: args
  end
end
```

```bash
#!/bin/bash

set -euo pipefail

echo "=== Install k3s ==="

curl -sfL https://get.k3s.io | K3S_KUBECONFIG_MODE="644" sh -s -

echo "=== Successfully install k3s ==="
```

# Kubernetes Dashboard

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.2/aio/deploy/recommended.yaml
```

```bash
kubectl proxy
```
