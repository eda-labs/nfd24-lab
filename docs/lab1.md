---
hide: navigation
---

# The Lab Environment

We've got a 10 device setup, 8 leafs and 2 spines.  The topology is already deployed (Fun fact, all the devices are running virtually on containerized!).

We're going to build out DC fabric on top of this topology through out this lab!

# Lab 1: Using a Declaritive Approach to Network Provisioning

As you may have figured out by now, K8s was our guiding light to building a truely declaritive automation platform - and so in this lab we'll use K8s and its tooling to discover what this means.

You're probably wondering, "Why on earth should I venture into the world of Kubernetes (aka: K8s) to manage my data center infrastructure, like my fabric?"

Well, the good news is that you don't need to, but that doesn't mean you shouldn't! With EDA, there is absolutely no requirement to know how to manage Custom Resources or really use K8s at all.  You can use our UI or even our REST API if you're feeling adventurous. However, as you look to modernize your network automation toolchain, we believe you may find that the success of K8s has yielded a large number of community and enterprise integrations/tools, which you may find useful. And hey, you may even end up speaking the same language as your application folks, if that's of any interest, of course.

Let's take a look at what it may look like to use K8s and some of its tooling to deploy a Fabric.

## Step 1: Creating a Fabric using Kubectl

SSH into your assigned lab environment if you haven't already:

<<<<<<< HEAD
| Connection | URL/Command                       | Example                |
| ---------- | --------------------------------- | ---------------------- |
| SSH        | `ssh nfd@nfd`**`<id>`**`@srexperts.net` | `ssh nfd@nfd1.srexperts.net`  |
=======
| Connection   | URL/Command                             | Example                          | Password        |
| ------------ | --------------------------------------- | -------------------------------- | --------------- |
| SSH          | `ssh nfd@nfd`**`<id>`**`@srexperts.net` | `ssh nfd@nfd1.srexperts.net`     | `nfd+eda@nokia` |
| Web Terminal | <https://go.srlinux.dev/ac1ssh{ID}>     | <https://go.srlinux.dev/ac1ssh1> |
>>>>>>> 808ef586b6a66862c9d86f5281bf8029af4aec97

Kubectl is the de facto CLI tool for all things K8s. It can be used for interacting with a cluster and its resources.

Here is the K8s Custom Resource definition for deploying the configuration needed to end up with a functional fabric. This is our desired intent!

You can copy the YAML provided below to your terminal using your favorite text editor and then use kubectl to apply the newly created YAML file. You can take a look at the kubectl tab below and just copy past that entire command to do it all for you!

<<<<<<< HEAD
Feel free to switch to the kubectl tab to copy paste the command into your terminal and apply this Fabric - in a declaritive way.
=======
1. Copy content of below to the terminal using your favorite editor.
2. `kubectl apply -f <new file you just created>`
>>>>>>> 808ef586b6a66862c9d86f5281bf8029af4aec97

/// tab | YAML

```yaml
--8<-- "lab1/fabric_1.yaml"
```

///
/// tab | `kubectl`

```bash
cat << 'EOF' | tee my-fabric.yaml | kubectl apply -f -
--8<-- "lab1/fabric_1.yaml"
EOF
```

///

How easy was that? But what actually happened?

The Fabric app just went and created a bunch of lower level abstractions that make a DC fabric.  ISLs, BGP peers, BGP groups, interfaces etc  These are all abstractions which were created by the Fabric app and then eventually those abstractions created node configuration and pushed them to your topology!

You can also use kubectl to view the status of your newly created Fabric and all the lower level abstractions it created!

```
kubectl get isl
```

```
kubectl get DefaultBGPGroup bgpgroup-ebgp-sunnyvale-dc1 -o yaml
```

## Step 2: Trust but verify with K9s

Lets verify something actually happened, but this time lets use another K8s tool - a popular CLI UI called K9s.

From your terminal simply type in `k9s` to launch the UI.

You will be shown a list of K8s pods, lets take a look at our newly deployed Fabric by pressing `:` followed by typing `fabrics` and pressing `enter`.

<iframe width="100%" height="600px" src="https://www.youtube.com/embed/Zm3el3psFrw" title="k9s fabric" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Note its operational state.  The Fabric emits multiple other abstractions such as Interswitch Links (ISLs), lets take a look at those!

Press `:` followed by `isls` and `enter`

To see more details about a particular ISL, press `d` while one of the ISL is selected

<iframe width="100%" height="600px" src="https://www.youtube.com/embed/KpuCeAQ6aMk" title="k9s isl" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Step 3 (Optional): Clicky click with Headlamp UI

While DevOps folks tend to prefer CLI tools and text editors, the UI world is also a great place to be. Again, leveraging the K8s-powered open source ecosystem we deployed the Headlamp UI in our cluster to provide a web-based UI for interacting with the cluster.

To access the UI you have to fetch the login token that is patiently waiting for you in your terminal:

```
[*]─[nfd1]─[~]
└──> cat ~/hl-token
eyJhbGciOiJSUzI1NiIsImtpzU3U0ljZld6Uk1tZEhwdnMifQ...
```

Copy the token and open up the Headlamp UI in a browser by visiting the following HTTP URL:

| Connection | URL/Command                         | Example                          |
| ---------- | ----------------------------------- | -------------------------------- |
| Web        | `http://nfd<ID>.srexperts.net:4466` | <http://nfd1.srexperts.net:4466> |

When prompted for the token, paste the token you copied earlier.

Using the Headlamp K8s web UI lets make a change to our Fabric resource and see its impact on DefaultBGPPeers.

<<<<<<< HEAD
| Connection | URL/Command                       | Example                |
| ---------- | --------------------------------- | ---------------------- |
| Web        | `http://nfd`**`<id>`**`.srexperts.net:4466` | <http://nfd1.srexperts.net:4466> |

The token you need to use to login is found in the file called `hl-token` found via the SSH session.

```
cat hl-token
```

Lets change the pool used for the Interswitch Links from IPv4 to IPv6.  
=======
Lets change the pool used for the Interswitch Links from IPv4 to IPv6.
>>>>>>> 808ef586b6a66862c9d86f5281bf8029af4aec97

<iframe width="100%" height="600px" src="https://www.youtube.com/embed/zEKzGZa9CK4" title="head" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
