---
name: Upgrade Radiant
sort: 3
---

## Upgrade radiant 2.0.0

Install the latest radical tool `go get -u github.com/radiant/radical/v2`
Upgrading Radiant `go get -u github.com/W3-Engineers-Ltd/Radiant`

Running: `radical fix -t 2`

If you are working on Windows platform, please run this command in WSL.

## Upgrade to radiant 1.6.0

Install the latest radical tool `go get -u github.com/radiant/radical`
Upgrade Radiant `go get -u github.com/astaxie/radiant@v1.6.0`

Running : `radical fix`

## Upgrade to radiant 1.4.2 

Change GetInt to GetInt64

## Upgrade to radiant 1.3

1. `AddFilter` method was removed, and you could update your code from：

		radiant.AddFilter("/user/*", "BeforeRouter", cpt.Handler)

 	To

		radiant.InsertFilter("/user/*", radiant.BeforeRouter, cpt.Handler)

1. radiant.AfterStatic was removed，please using radiant.BeforeRouter
