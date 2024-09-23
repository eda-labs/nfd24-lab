---
hide: navigation
---

# The Lab Environment

We have a 10-device setup, consisting of 8 leaf switches and 2 spines. The topology is already deployed (Fun fact: all the devices are running in containers!).

We're going to build a data center (DC) fabric on top of this topology throughout this lab!

# Lab 1: Using a declarative approach to network provisioning

As you may have figured out by now, Kubernetes (K8s) was our guiding light in building a truly declarative automation platform—so in this lab, we'll use K8s and its tools to explore what this means.

You're probably wondering, "Why on earth should I venture into the world of Kubernetes (aka K8s) to manage my data center infrastructure, like my fabric?"

Well, the good news is that you don't need to—but that doesn't mean you shouldn't! With EDA, there is absolutely no requirement to know how to manage Custom Resources or even use K8s at all. You can use our UI or even our REST API if you're feeling adventurous. However, as you modernize your network automation toolchain, you may find that the success of K8s has brought a large number of community and enterprise integrations/tools, which you might find useful. And hey, you may even end up speaking the same language as your application folks, if that's of any interest, of course.

Let's take a look at what it may look like to use K8s and some of its tools to deploy a fabric.

## Step 1: Creating a Fabric using Kubectl

SSH into your assigned lab environment if you haven't already:

| Connection   | URL/Command                             | Example                          | Password        |
| ------------ | --------------------------------------- | -------------------------------- | --------------- |
| SSH          | `ssh nfd@nfd`**`<id>`**`.srexperts.net` | `ssh nfd@nfd1.srexperts.net`     | `nfd+eda@nokia` |
| Web Terminal | <https://go.srlinux.dev/ac1ssh{ID}>     | <https://go.srlinux.dev/ac1ssh1> |

Kubectl is the de facto CLI tool for all things K8s. It can be used to interact with a cluster and its resources.

Here is the K8s Custom Resource definition for deploying the configuration needed to create a functional fabric. This is our desired intent!

You can use the kubectl tab below and copy-paste the command to do it all for you!


/// tab | `kubectl`

```bash
cat << 'EOF' | tee my-fabric.yaml | kubectl apply -f -
--8<-- "lab1/fabric_1.yaml"
EOF
```

///
/// tab | YAML

```yaml
--8<-- "lab1/fabric_1.yaml"
```

///


How easy was that? But what actually happened?

The Fabric app just created a bunch of lower-level abstractions that make up a DC fabric—ISLs, BGP peers, BGP groups, interfaces, etc. These abstractions were created by the `Fabric` app and then translated into node configurations, which were pushed to your topology!

You can also use `kubectl` to view the status of your newly created `Fabric` and all the lower-level abstractions it generated!

```
kubectl get isl
```

```
kubectl get DefaultBGPGroup bgpgroup-ebgp-sunnyvale-dc1 -o yaml
```

## Step 2: Trust but verify with K9s

Let's verify that something actually happened, but this time let's use another K8s tool—a popular CLI UI called K9s.

From your terminal, simply type `k9s` to launch the UI.

You will be shown a list of K8s pods. Let’s take a look at our newly deployed `Fabric` by pressing `:` followed by typing `fabrics` and pressing `enter`.

<iframe width="100%" height="600px" src="https://www.youtube.com/embed/Zm3el3psFrw" title="k9s fabric" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
Note its operational state. The Fabric emits multiple other abstractions such as Interswitch Links (ISLs); let's take a look at those!

Press `:` followed by `isls` and press `enter`.

To see more details about a particular ISL, press `d` while one of the ISLs is selected.

<iframe width="100%" height="600px" src="https://www.youtube.com/embed/KpuCeAQ6aMk" title="k9s isl" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Step 3 (Optional): Clicky click with Headlamp UI

While DevOps folks tend to prefer CLI tools and text editors, the UI world is also a great place to be. Again, leveraging the K8s-powered open-source ecosystem, we deployed the Headlamp UI in our cluster to provide a web-based UI for interacting with the cluster.

To access the UI, you need to fetch the login token that is patiently waiting for you in your terminal:

```
cat ~/hl-token
```

The output should be a string like this:

```
eyJhbGciOiJSUzI1NiIsImtpZCI6IjBmcmdvOWJtc2wtSlA1S2RhcDBrOTIwS3hUVzU3U0ljZld6Uk1tZEhwdnMifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNzI3MDg1MDUwLCJpYXQiOjE3MjcwODE0NTAsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJoZWFkbGFtcC1hZG1pbiIsInVpZCI6IjNhZTljMjBkLWEzMWItNGE1Yy1iZTMxLTBiNDZiMTczNmU1NyJ9fSwibmJmIjoxNzI3MDgxNDUwLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06aGVhZGxhbXAtYWRtaW4ifQ.XbriCIhAtlbhfCDm8D330eng_x66K49x43oOlhDbmGBeJ89mDveV1EkovsE_i7hRlYjHZP3E-UgMrugXsFQ3oDXuYmFq0fLhFgbQzfF5CBG4DpLb1_bA-dX30MNDE0n7RRn0xKJWqHmqlTVnxR9fVJb8ooJ56ll5RxnCP4_6i8pE_oQ_7dDIZyGyZ1QP5_86DuJXPE81HBx4kSuVTCCOx3hM5LNhLFOjBhenqcyk36mQMr39Nbr9FumKxZ_vktzt30YtUCrDWJG-pnRPzpvK5_2OgCYat5-uul5dR1VHanuf-zS1OOUJt_sTa3zXJbSsyQ4Cl1arTqNAJU5ktMpaoA
```

Copy the token and open up the Headlamp UI in a browser by visiting the following HTTP URL:

| Connection | URL/Command                         | Example                          |
| ---------- | ----------------------------------- | -------------------------------- |
| Web        | `http://nfd<ID>.srexperts.net:4466` | <http://nfd1.srexperts.net:4466> |

When prompted for the token, paste the token you copied earlier.

Using the Headlamp K8s web UI lets make a change to our Fabric resource and see its impact on DefaultBGPPeers.

Lets change the pool used for the Interswitch Links from IPv4 to IPv6.

<iframe width="100%" height="600px" src="https://www.youtube.com/embed/zEKzGZa9CK4" title="head" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
