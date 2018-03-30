
# Architecture

This document describes the different types of scenarios in which Masq can operate. By default, all data transmitted between devices is encrypted using [Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy), independent of whether the Sync Server uses TLS or not. More cryptographic details can be found in the [Sync document](https://github.com/QwantResearch/masq-docs/blob/gh-pages/sync).


## TOC

### [Terminology](#terminology)
### [Architecture](#architecture)
### [Scenarios](#scenarios)
* [Multiple stores](#scenario-1---multiple-stores)
* [Store and client apps](#scenario-2---store-and-client-apps)
* [No centralised store](#scenario-3---no-centralised-store)

## Terminology

**Peer** - a device (e.g. desktop computer, laptop, smartphone, tablet, etc.) that is under the user's physical control.

**Store** - program/service that stores and retrieves application data. Think of it as a local database on the user's device.

**Store manager** - User Interface that manages pairing and sync preferences for a store.

**Application** - a native application or a Web app running the browser. In specific cases (i.e. smartphones/tablets) it may be directly considered as a *peer*.

**Sync service** - A third-party service that connects multiple peers, acting only as a relay that passes messages along without storing any kind of data.

## Architecture

![Store - Store](https://qwantresearch.github.io/masq-docs/img/arch.png)

Masq allow applications to store and synchronise data without relying on a cloud-based solution. It typically does this by offering on-device storage, either through a standalone (desktop) application that a user installs, or by relying on HTML5 storage APIs available in the browsers today.

The figure presented above showcases two peers that are synchronised through a dedicated sync service. The first peer consists of a desktop computer (any personal computer will do) while the second peer is a mobile device (a smartphone in this case). The sync data is secured in transit using two layers of encryption. One, connections to the *Sync service* use TLS by default. And two, data is encrypted on the device using [Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) before being sent over the wire. FPS is the same protocol that chat applications like Signal and WhatsApp use.

Running Masq as a standalone application on a personal computer offers a significant number of advantages. For instance, data is centralised in one place instead of being duplicated for each app or different browser that runs on the same device. A standalone store in the form of a desktop app also has access to the whole space available on the device instead of only 10% that is usually available to the browser.

To connect to the data store, an app will use the client API offered by our [MasqClient](https://github.com/QwantResearch/masq-client) library. All data transfers will go through a local WebSocket connection. If a native store app is not available (i.e. MasqClient cannot open a connection to the `localhost` store), the library will fallback on a peer-2-peer mode and has to be individually paired with the rest of user's devices as if it was a device. In this case, MasqClient will use TLS to open a **secure** WebSocket connection directly with the *Sync service*.


## Scenarios

### Scenario 1 - multiple stores

This scenario fits personal computers and/or servers and it involves a centralised store on each peer. The store can be a standalone application that listens to incoming connections from local applications (either browser-based on native), and handles pairing and sync operations for all the apps. Typically, a Web app would use the `WebSocket` protocol to store and retrieve application data from a local store service listening on `localhost`. The store would then also use a `WebSocket` connection to synchronise changes with other (previously paired) devices using a *Sync Server*.

The main advantage of using a local store is that the user only has to manage pairing/sync preferences in one place, regardless of how many applications are used on the computer. Also, it avoids duplicating data, especially when using the same app in multiple browsers on the same machine.

![Store - Store](https://qwantresearch.github.io/masq-docs/img/store-store.png)

### Scenario 2 - store and client apps

This scenario usually involves personal computers and mobile devices, on which a centralised store cannot be deployed due to constraints imposed by the mobile OS. The problem in this example is that the application has to be paired directly with the the other devices, instead of allowing it to connect to a store that runs on the same device, and which handles the pairing and sync operations.

![Store - App](https://qwantresearch.github.io/masq-docs/img/store-app.png)

### Scenario 3 - no centralised store

The final scenario consists of connecting applications running on different mobile devices. This is the worst possible outcome, since applications have to be individually paired between devices, due to no centralised management of pairing/sync preferences.

![App - App](https://qwantresearch.github.io/masq-docs/img/app-app.png)

