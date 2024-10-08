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
<!-- --8<-- [start:connectivity] -->
| Connection | URL/Command                             | Example                                             | :fontawesome-solid-user-secret: PaS$w0яd |
| ---------- | --------------------------------------- | --------------------------------------------------- | ---------------------------------------- |
| SSH        | `ssh nfd@nfd`**`<id>`**`.srexperts.net` | `ssh nfd@nfd1.srexperts.net`                        | user: `nfd`<br/>pass: `nfd+eda@nokia`    |
| EDA UI     | `https://nfd`**`<id>`**`.srexperts.net` | <https://nfd1.srexperts.net>{ data-proofer-ignore } | user: `admin`<br/>pass: `nfd+eda@nokia`  |
<!-- --8<-- [end:connectivity] -->

As crazy as it may sound, it is 2024 and we still ask you to have your terminals dusted off and ready to SSH. Not because we are old-fashioned, but because the Infrastructure as Code movement has _code_ in the name not for nothing.

/// details | :scream: I don't have a terminal emulator
    type: danger
That is unfortunate, but fear not! We have prepared a web based terminals for each and one of you.

Copy a link below with substituting the **`<id>`** with your delegate id.

```
https://go.srlinux.dev/ac1ssh<id>
```

Once the web terminal is open, click on the :octicons-plus-circle-24: button and you'll drop down in a beautiful shell of your sandbox environment.

///

## The Lab Environment

We have a 12-nodes setup, consisting of 8 leaf switches, 2 spines and 2 border leafs. The topology is already deployed (Fun fact: all the devices are running in containers!).

<figure>
  <div class='mxgraph' style='max-width:100%;border:1px solid transparent;margin:0 auto; display:block;box-shadow: 0 25px 50px -12px rgb(0 0 0 / 0.25);border-radius: 0.25rem;' data-mxgraph='{"page":0,"zoom":2,"highlight":"#0000ff","nav":true,"resize":true,"edit":"_blank","url":"https://raw.githubusercontent.com/eda-labs/nfd24-lab/main/topo.drawio"}'></div>
  <figcaption>Lab topology</figcaption>
</figure>

We're going to start our journey by building a data center (DC) fabric on top of this topology in the first lab exercise!

This is all we ask of you, lets jump right into the first lab!

[:octicons-arrow-right-24: Lab1](lab1.md)

<script type="text/javascript" src="https://viewer.diagrams.net/js/viewer-static.min.js" async></script>
