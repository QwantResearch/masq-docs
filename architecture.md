
#Masq Architecture

This document describes the different types of configuration in which Masq can operate.

### Example 1 - multiple stores

This example fits personal computers and/or servers and it involves a centralised store. The store can be a standalone application that listens to incoming connections from local applications (either browser-based on native). Typically, a Web app would use the `WebSocket` protocol to store and retrieve application data from a local store service listening on `localhost`. The store would then also use a `WebSocket` connection to synchronise changes with other (previously paired) devices using a *Sync Server*.

![Store - Store](https://qwantresearch.github.io/masq-docs/img/store-store.png)

### Example 2 - store and client apps

This example usually involves mobile devices, where a centralised store cannot be deployed due to constraints imposed by the mobile OS.

![Store - App](https://qwantresearch.github.io/masq-docs/img/store-app.png)

### Example 3 - no centralised store

![App - App](https://qwantresearch.github.io/masq-docs/img/app-app.png)

## Clients

We can consider Masq clients as being either a standalone application running on a computer, or an application that has been paired with other clients.
