---
hide: navigation
---

# Lab 1: Kubernetes and its Vast Toolchain

You're probably wondering, "Why on earth should I venture into the world of Kubernetes (aka: K8s) to manage my data center infrastructure, like my fabric?"

Well, the good news is that you don't need to, but that doesn't mean you shouldn't! With EDA, there is absolutely no requirement to know how to manage Custom Resources or really use K8s at all. You can use our UI or even our REST API if you're feeling adventurous. However, as you look to modernize your network automation toolchain, we believe you may find that the success of K8s has yielded a large number of community and enterprise integrations/tools, which you may find useful. And hey, you may even end up speaking the same language as your application folks, if that's of any interest, of course.

Let's take a look at what it may look like to use K8s and some of its tooling to deploy a Fabric.

## Step 1: Creating a Fabric using Kubectl

SSH into your assigned lab environment if you haven't already:

| Connection | URL/Command                       | Example                |
| ---------- | --------------------------------- | ---------------------- |
| SSH        | `ssh nfd@nfd`**`<id>`**`@eda.dev` | `ssh nfd@nfd.eda.dev`  |

Kubectl is the de facto CLI tool for all things K8s. It can be used for interacting with a cluster and its resources.

Here is the K8s Custom Resource definition for deploying the configuration needed to end up with a functional fabric. This is our desired intent!

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

## Step 2: Trust but verify with K9s

Lets verify something actually happened, but this time lets use another K8s tool - a popular CLI UI called K9s.

From your terminal simply type in `k9s` to launch the UI.

You will be shown a list of K8s pods, lets take a look at our newly deployed Fabric by pressing `:` followed by typing `fabrics` and pressing `enter`.

<iframe width="1222" height="843" src="https://www.youtube.com/embed/Zm3el3psFrw" title="k9s fabric" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Note its operational state.  The Fabric emits multiple other abstractions such as Interswitch Links (ISLs), lets take a look at those!

Press `:` followed by `isls` and `enter`

To see more details about a particular ISL, press `d` while one of the ISL is selected

<iframe width="1222" height="843" src="https://www.youtube.com/embed/KpuCeAQ6aMk" title="k9s isl" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Step 3 (Optional): Clicky click with Headlamp UI

Using the Headlamp K8s web UI lets make a change to our Fabric resource and see its impact on DefaultBGPPeers.

Lets change the pool used for the Interswitch Links from IPv4 to IPv6.  

<iframe width="1222" height="989" src="https://www.youtube.com/embed/zEKzGZa9CK4" title="head" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>