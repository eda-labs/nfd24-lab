---
hide: navigation
---

# Welcome, NFD delegates

Nokia welcomes you to the Event Driven Automation (EDA) Hands-On Lab Experience.  
The carefully selected delegates will be among the first to experience the latest Nokia developments in the field of the infrastructure automation with a strong focus on the networking domain.

Most likely you've already been administered a non-lethal dose of the EDA's core concepts, and now it is time to put those concepts into practice.  
We have prepared a series of hands-on labs that puts you into the seat of a network automation engineer working with EDA. Each of you is assigned their own sandboxed EDA environment so that you can work at your own pace without someone's breathing down your neck. Of course, you are free to team up, that's even better; the choice is yours.

All you need is a web browser and a terminal emulator.

## Connectivity

Each of you has been assigned a delegate `id`; using this identifier, you can access your personal EDA environment:

| Connection | URL/Command                       | Example                |
| ---------- | --------------------------------- | ---------------------- |
| SSH        | `ssh nfd@nfd`**`<id>`**`@srexperts.net` | `ssh nfd@nfd1.srexperts.net`  |
| Web        | `https://nfd`**`<id>`**`.srexperts.net` | <https://nfd1.srexperts.net> |

As crazy as it may sound, it is 2024 and we will still ask you to have your terminals dusted off and ready to SSH. Not because we are old-fashioned, but the Infrastructure as Code movement has been around for a while and we want you to try this yourself.

/// details | :scream: I don't have a terminal emulator
    type: danger
That is unfortunate, but fear not! We have prepared a web based terminals for each and one of you.

Copy a link below with substituting the **`<id>`** with your delegate id.

```
https://go.srlinux.dev/ac1ssh<id>
```

Once the web terminal is open, click on the :octicons-plus-circle-24: button and you'll drop down in a beatiful shell of your sandbox environment.

///

## Let's go

This is all we ask of you, lets jump right into the first lab

[:octicons-arrow-right-24: Lab1](lab1.md)
