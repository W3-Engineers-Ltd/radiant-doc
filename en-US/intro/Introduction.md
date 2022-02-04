---
root: true
name: Radiant introduction
sort: 0
---

# What is Radiant?

Radiant is a RESTful HTTP framework for the rapid development of Go applications including APIs, web apps and backend services with integrated Go specific features such as interfaces and struct embedding.

## The architecture of Radiant

Radiant is built upon 8 loosely linked modules that can be used independently or as part of Radiant's HTTP logic. This high level of modularity gives Radiant an unprecedented level of flexibility to meet developer needs.

![](../images/architecture.png)

## The execution logic of Radiant

Radiant uses a standard Model-View-Controller (MVC) architecture for logic execution.

![](../images/flow.png)

## The project structure of Radiant

Here is the typical folder structure of a Radiant project:

```
├── conf
│   └── app.conf
├── controllers
│   ├── admin
│   └── default.go
├── main.go
├── models
│   └── models.go
├── static
│   ├── css
│   ├── ico
│   ├── img
│   └── js
└── views
    ├── admin
    └── index.tpl
```

M (models), V (views), C (controllers) each have top level folders. `main.go` is the entry point.

## Creating a Radiant project

Ready to try Radiant?  You can use the [radical tool to create a new project](../install/radical.md).
