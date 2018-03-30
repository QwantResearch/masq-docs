
# Architecture

This document describes the different types of scenarios in which Masq can operate. By default, all data transmitted between devices is encrypted using [Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy), independent of whether the Sync Server uses TLS or not. More cryptographic details can be found in the [Sync document](https://github.com/QwantResearch/masq-docs/blob/gh-pages/sync).


## TOC

### 1. [Terminology](#terminology)
### 2. [Scenarios](#scenarios)
* [Multiple stores](#scenario-1---multiple-stores)
* [Store and client apps](#scenario-2---store-and-client-apps)
* [No centralised store](#scenario-3---no-centralised-store)

## Terminology

**Peer** - a device (e.g. desktop computer, laptop, smartphone, tablet, etc.) that is under the user's physical control.

**Store** - program/service that stores and retrieves application data. Think of it as a local database on the user's device.

**Store manager** - User Interface that manages pairing and sync preferences for a store.

**Application** - a native application or a Web app running the browser. In specific cases (i.e. smartphones/tablets) it may be directly considered as a *peer*.

**Sync server** - A third-party server running a synchronisation service that connects multiple peers.

## Scenarios

![Store - Store](https://qwantresearch.github.io/masq-docs/img/arch.svg)

### Scenario 1 - multiple stores

This scenario fits personal computers and/or servers and it involves a centralised store. The store can be a standalone application that listens to incoming connections from local applications (either browser-based on native), and handles pairing and sync operations for all the apps. Typically, a Web app would use the `WebSocket` protocol to store and retrieve application data from a local store service listening on `localhost`. The store would then also use a `WebSocket` connection to synchronise changes with other (previously paired) devices using a *Sync Server*.

The main advantage of using a local store is that the user only has to manage pairing/sync preferences in one place, regardless of how many applications are used on the computer. Also, it avoids duplicating data, especially when using the same app in multiple browsers on the same machine.

![Store - Store](https://qwantresearch.github.io/masq-docs/img/store-store.png)

### Scenario 2 - store and client apps

This scenario usually involves personal computers and mobile devices, on which a centralised store cannot be deployed due to constraints imposed by the mobile OS. The problem in this example is that the application has to be paired directly with the the other devices, instead of allowing it to connect to a store that runs on the same device, and which handles the pairing and sync operations.

![Store - App](https://qwantresearch.github.io/masq-docs/img/store-app.png)

### Scenario 3 - no centralised store

The final scenario consists of connecting applications running on different mobile devices. This is the worst possible outcome, since applications have to be individually paired between devices, due to no centralised management of pairing/sync preferences.

![App - App](https://qwantresearch.github.io/masq-docs/img/app-app.png)

