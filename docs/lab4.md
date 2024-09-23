---
hide: navigation
---

# Lab 4: Tell the world about EDA

Imagine having a powerful system that knows everything about your network—every event, every alert, all the fine details—but it can’t communicate with anyone else. Frustrating, right? It’s like completing a massive puzzle, but never showing anyone the finished picture!

That’s why integration is key, and the Notifier app steps in. It bridges the gap, making sure that all those critical events and conditions are delivered exactly where they need to go, ensuring your network’s intelligence isn’t just locked away but actually working for you.

/// admonition | Apps?
    type: subtle-question
Almost everything in EDA is considered an Application (App), for example the Fabrics app that you interacted with in [Lab 1](lab1.md) or VNET app in [Lab 2](lab2.md).  
Apps extend the core functionality of EDA by introducing new declarative abstractions, intergrations with other systems, and more.

///

## The Notifier app

Notifier lets you craft and deliver personalized alerts tailored to your specific events and conditions.
Whether you need straightforward alerts or more customized notifications using the power of EQL (EDA Query Language),
the Notifier app gives you full control over how and when notifications are triggered.

## Step1: Install it

Apps can be installed using the App Store UI or by applying a resource directly via any of the programmable interfaces.  
The App Store method is great for "shopping" around for apps, while applying a YAML resource allows you to automate this process at will.

Here is how to install the notifier app using the App Store UI.

/// tab | Step 1
Select the <kbd>System Administration</kbd> item in the left menu of the EDA UI.

![p1](https://gitlab.com/rdodin/pics/-/wikis/uploads/c73335f0cefc916eb9bb78a61519faeb/image.png){width=50%}
///
/// tab | Step 2
You will find yourself in the App Store UI.

In the filter menu select the <kbd>Catalog</kbd> drop down menu and select <kbd>3rd Party Catalog</kbd>.

![p2](https://gitlab.com/rdodin/pics/-/wikis/uploads/1dcbd1880ab509a218d08fb4fe45f2de/image.png){width=80%}
///
/// tab | Step 3
Select the notifier app card to open up the application page.

![p3](https://gitlab.com/rdodin/pics/-/wikis/uploads/bca4b67264938d4588867baf33ce2672/image.png){width=50%}
///
/// tab | Step 4
Press Install and wait a couple of seconds until the app is installed.

![p4](https://gitlab.com/rdodin/pics/-/wikis/uploads/49a7f44ca1c4af3171e6a24486c98226/image.png)
///

//// details | CLI installation option
Alternatively, applying the below resource installs of the notifier app.

You can copy the entire command in the 1st tab and paste it in the terminal of your VM.

/// tab | `kubectl apply` command

```bash
kubectl apply -f - <<'EOF'
--8<-- "docs/notifier/notifier_appinstall.yml"
EOF
```

///

/// tab | YAML Resource

```yaml
--8<-- "docs/notifier/notifier_appinstall.yml"
```

///
////

## Step2: Use it

The `Notifier` app works with two key resources: a `Provider` and a `Notifier`.

The `Provider` is a notification destination such as Discord, Team, Email, etc.
While the `Notifier` defines the content of the notification and the specific conditions that trigger it.
Together, they ensure your notifications go exactly where they’re needed, with the right information.

### Step2.1: Alarm-based notifier

An alarm-based notifier sends notifications triggered by alarms raised by EDA. You can choose to include or exclude specific alarms, ensuring you only get the alerts you need. Plus, you’ll be notified when those alarms are cleared, keeping everything neat and tidy.

#### Configure a Discord Provider

To create a `Provider` using the UI:

* Switch back to the <kbd>Main</kbd> panel if you're still in the App Store UI.
* Navigate to the `Notifier` resource category, select `Provider` and press __Create__ at the top right corner of the UI.
* Fill in the `Provider` form by giving the `Provider` a name. For instance - `discord` - since we will be sending notifications to a Discord channel.
* Under the Specification section, set the `URI` to:

    ```
    discord://yQeo_GQx3enCixFzlxE1UetZAUnSF9isl6rBfkBgb9vuurlgomstbhPpoyJryAMeLdby@1280864861592485900
    ```

    The URI format is `discord://token@webhookid` where `token` and `webhookid` are extracted from the webhook URL that Discord generates for you.

    ```text
    https://discord.com/api/webhooks/webhookid/token
                                    ^^^^^^^^^ ^^^^^  
    ```

* Press __Commit__. The Provider will be applied to the cluster and will appear in the list of `Providers`

//// details | CLI method
Alternatively, applying the below resource creates the same discord provider.

You can copy the entire command in the 1st tab and paste it in the terminal of your VM.

/// tab | `kubectl apply` command

```bash
kubectl apply -f - <<'EOF'
--8<-- "docs/notifier/provider.yml"
EOF
```

///

/// tab | YAML Resource

```yaml
--8<-- "docs/notifier/provider.yml"
```

///
////

#### Define a simple Alarm notifier

To create an alarm-based notifier:

* Navigate to the `Notifier` resource category, select `Notifier` and press __Create__ at the top right corner of the UI.
* Fill in the `Notifier` form by giving the `Notifier` a name. For example - `alarms-to-discord` - since we will be sending alarms to a Discord channel.
* Under `Providers` add an item and set it to `discord`. That's the name of provider created in the previous step.
* Under `Specification | Sources | Alarms`, add an `Include` item and set it to `*`. That means include all alarms.
* Press __Commit__. The Notifier will be applied to the cluster and will appear in the list of `Notifiers`

//// details | CLI method
Alternatively, applying the below resource creates the same alarm notifier.

You can copy the entire command in the 1st tab and paste it in the terminal of your VM.

/// tab | `kubectl apply` command

```bash
kubectl apply -f - <<'EOF'
--8<-- "docs/notifier/alarm_notifier.yml"
EOF
```

///

/// tab | YAML Resource

```yaml
--8<-- "docs/notifier/alarm_notifier.yml"
```

///

////

#### Test the notifier

To test the notifier the easiest way is to trigger an alarm. For this lab we simply disable an interface.

* Navigate to the resources category `Topology` and select `Interfaces`.
* Double click on any interface you like and toggle the switch that says `Enabled`.
* Press __Commit__ at the button right of the form.
* Check the notifications sent to the Discord Channel.
* Enable back the interface. Check the notifications sent in the Channel.

### Step2.2: Query-based notifier

A query-based notifier leverages the full power of EDA Query Language, letting you precisely choose which objects and conditions trigger your notifications. You can also customize the notification’s title and body using templates, giving you complete control over the details.

We will define a notifier that gets triggered when a new LLDP neighbor is discovered and reuse the same Discord Provider defined in the previous step.

#### Configuring query-based notifier

* Navigate to the `Notifier` resource category, select `Notifier` and press __Create__ at the top right corner of the UI.
* Fill in the `Notifier` form by giving the `Notifier` a name. For example - `new-lldp-neighbor` - since we will be sending alarms to a Discord channel.
* Under `Providers` add an item and set it to `discord`. That's the name of provider created in the previous step.
* Under `Specification | Sources | Query`, set `Table` to

    ```
    .db.cr-status.interfaces_eda_nokia_com.v1alpha1.interface.status.members.neighbors
    ```

* Add three `Fields` values in order for us to later use them in the message templates:

    * `.db.cr-status.interfaces_eda_nokia_com.v1alpha1.interface.name`
    * `interface`
    * `node`

* Set `Title` to

    ```
    LLDP neighbor - {{ index . ".db.cr-status.interfaces_eda_nokia_com.v1alpha1.interface.name" }} -> {{ index . "node" }}-{{ index . "interface" }}
    ```

* Set `Template` to

    ```
    A new LLDP neighbor has appeared on interface {{ index . ".db.cr-status.interfaces_eda_nokia_com.v1alpha1.interface.name" }}: host name {{ index . "node" }}, interface name {{ index . "interface" }}
    ```

* Press __Commit__. The Notifier will be applied to the cluster and will appear in the list of `Notifiers`

//// details | CLI method
Alternatively, applying the below resource creates the same query notifier.

You can copy the entire command in the 1st tab and paste it in the terminal of your VM.

/// tab | `kubectl apply` command

```bash
kubectl apply -f - <<'EOF'
--8<-- "docs/notifier/lldp_neighbor_notifier.yml"
EOF
```

///

/// tab | YAML Resource

```yaml
--8<-- "docs/notifier/lldp_neighbor_notifier.yml"
```

///

////

#### Test the notifier

To test the notifier we need to add/remove an LLDP neighbor. For this lab we simply disable an interface.

* Navigate to the resources category `Topology` and select `Interfaces`.
* Double click on any interface you like and toggle the switch that says `Enabled`.
* Press __Commit__ at the button right of the form.
* Check the notifications sent to the Discord Channel.
* Enable back the interface. Check the notifications sent in the Channel.

## Step3 (optional): Customize it

Take it a step further by playing around with queries and templates to fine-tune your notifications.
The Notifier app allows for deep customization, so you can craft notifications that perfectly suit your needs.

Hints:

* BGP Neighbors state notifications:

    With the help of the Query page use the following query to build a BGP neighbor session state notifier
    `.node.srl.network-instance.protocols.bgp.neighbor where (session-state != "established")`

* Get some inspiration from the queries used in Lab3.
