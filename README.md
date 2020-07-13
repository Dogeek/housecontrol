# House Control

## Installation and requirements

This app requires 

* Kubernetes
* A cluster management system (minikube, gke, or microk8s for instance)
* Helm3
* Docker
* GNU make
* GNU pass && GPG

Services may be written in python (3.7+) or rust (2018), and each service will manage its own dependancies

Installation steps :

* First off, this app will only run on Linux machines. * Create a GPG key `gpg --quick-generate-key USERNAME` 
* Install pass and initialize the password store `pass init USERNAME` 
* Install Kubernetes, Docker and your cluster management system of choice 
* Start your cluster, i.e. `minikube start --driver=docker --cpus=4 --memory=8192`
* Run `eval $(minikube docker-env)` if you're using minikube
* Run `eval $(tools/setenv)`
* Run make on the base library
* Into each service, you can then run `make && make install` to install the services you want. The following are required
    * hc-login
    * hc-gateway
    * hc-frontend

## Goal

Make an all modular dashboard to control your domotics using kubernetes, docker and helm.

Each particular appliance will have its own service to control it.

In addition to the modularity cloud-native apps offer, the main dashboard will be split into several services as well :

* hc-login : Handles logging into the website
* hc-gateway : The gateway to the website. Aggregates the data from each module (service) into a rendered page
* hc-frontend : The main frontend of the app. Written using Vue.js

## Service architecture

Each service should have the following components :

* frontend : A Vue.js app which will render everything on the web page.
* service : The entry point for the kubernetes service.
* A Makefile to make the docker container and deploy using helm3
* Dockerfile to describe the container

You can use the `tools/boilerplate` tool to generate a blank service.
