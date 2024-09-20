---
hide: navigation
---

# Lab 4: Tell the world about EDA

Imagine having a powerful system that knows everything about your network—every event, every alert, all the fine details—but it can’t communicate with anyone else. Frustrating, right? It’s like completing a massive puzzle, but never showing anyone the finished picture!

That’s why integration is key, and the Notifier app steps in. It bridges the gap, making sure that all those critical events and conditions are delivered exactly where they need to go, ensuring your network’s intelligence isn’t just locked away but actually working for you.

## The Notifier app

Notifier lets you craft and deliver personalized alerts tailored to your specific events and conditions.
Whether you need straightforward alerts or more customized notifications using the power of EQL (EDA Query Language),
the Notifier app gives you full control over how and when notifications are triggered.

## Install it

Apps can be installed using the App Store or by applying a resource directly.
The App Store method is great for "shopping" around for apps, while applying a YAML resource allows you to automate this process at will.

1) App Store method

* Navigate to __System Administration__ in the top left of the UI.
* In the Catalog drop down menu select `3rd Party Catalog`.
* Select the notifier app version `v1.0.3`.
* Press Install.

2) Alternatively, applying the below resource triggers the installation of the notifier app:

/// tab | YAML Resource
```yaml
--8<-- "docs/notifier/notifier_appinstall.yml"
```
///

/// tab | `kubectl apply` command
```bash
kubectl apply -f - <<'EOF'
--8<-- "docs/notifier/notifier_appinstall.yml"
EOF
```
///

## Use it

The `Notifier` app works with two key resources: a `Provider` and a `Notifier`.

The `Provider` is a notification destination such as Discord, Teams Email, etc.
While the `Notifier` defines the content of the notification and the specific conditions that trigger it. Together, they ensure your notifications go exactly where they’re needed, with the right information.

### Alarm-based notifier

An alarm-based notifier sends notifications triggered by alarms raised by EDA. You can choose to include or exclude specific alarms, ensuring you only get the alerts you need. Plus, you’ll be notified when those alarms are cleared, keeping everything neat and tidy.

1) Configure a Discord Provider.

A Provider a notification destination such as Discord, Teams, Email, etc.

/// tab | YAML Resource
```yaml
--8<-- "docs/notifier/provider.yml"
```
///

/// tab | `kubectl apply` command
```bash
kubectl apply -f - <<'EOF'
--8<-- "docs/notifier/provider.yml"
EOF
```
///

2) Define a simple Alarm notifier

A notifier sets the object and conditions to trigger a notification.

The below example triggers notifications based on any received alarm.

/// tab | YAML Resource
```yaml
--8<-- "docs/notifier/alarm_notifier.yml"
```
///

/// tab | `kubectl apply` command
```bash
kubectl apply -f - <<'EOF'
--8<-- "docs/notifier/alarm_notifier.yml"
EOF
```
///

3) Test the notifier

// TODO: bounce an interface admin-state

### Query-based notifier

A query-based notifier leverages the full power of EDA Query Language, letting you precisely choose which objects and conditions trigger your notifications. You can also customize the notification’s title and body using templates, giving you complete control over the details.

1) We will reuse the same provider as in the previous example

2) Define an LLDP neighbor notifier

The below example triggers notifications whenever a new LLDP neighbor appears in an interface.

/// tab | YAML Resource
```yaml
--8<-- "docs/notifier/lldp_neighbor_notifier.yml"
```
///
/// tab | `kubectl apply` command
```bash
kubectl apply -f - <<'EOF'
--8<-- "docs/notifier/lldp_neighbor_notifier.yml"
EOF
```
///

## Customize it

Take it a step further by playing around with queries and templates to fine-tune your notifications.
The Notifier app allows for deep customization, so you can craft notifications that perfectly suit your needs.

// TODO: give some guidance 
