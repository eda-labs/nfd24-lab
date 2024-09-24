---
hide: navigation
---

# Lab 2: Truly declarative with transactionality

Need to add a VLAN to your data center? Push some new ACLs or BGP policies on your border leafs? Best not attempt that on a Friday in case something goes wrong. But what if it were safe to do so? What if you knew exactly what was going to happen before making the change and knew that if anything went wrong, the entirety of your network would be rolled back to a good working state?

Enter EDA, your network change savior, the ambassador of stress-free weekends!

In EDA, every configuration change is done in an all-or-nothing fashion and transacted in Git. Always. And declaratively! If the desired state is not achievable, even on a single device, the whole transaction is pronounced failed, and the changes are immediately reverted from all nodes.

The transactions are network-wide.

When you create any resource, EDA automatically initiates a transaction and publishes its result.

Transactions are one of the key components to ensure we reduce human error to zero. Let's dig in a bit deeper.

## Step 1: VLAN creation using transactions, what's the Diff?

Log into the EDA UI by visiting the following:

| Connection | URL/Command                             | Example                                             | :fontawesome-solid-user-secret: PaS$w0яd |
| ---------- | --------------------------------------- | --------------------------------------------------- | ---------------------------------------- |
| Web        | `https://nfd`**`<id>`**`.srexperts.net` | <https://nfd1.srexperts.net>{ data-proofer-ignore } | user: `admin`<br/>pass: `nfd+eda@nokia`  |

- Navigate to the Virtual Networks in the left-hand menu.
    ![vn](https://gitlab.com/rdodin/pics/-/wikis/uploads/6038ffed2e93d9f7cf0f014f995b043d/image.png)
- Double-click on the Virtual Network called `customer2` object in the table to open up a configuration form.

From here, let's add a VLAN to our storage network.

- You can use the filter in the top left of the form to help find the fields you are looking for. You can start typing `vlan`, for example. Once you've found the correct VLAN table, click on <kbd>Add</kdb>.

Fill out the form as follows:

- VLAN Name: `customer2-storage-network-vlan-1`
- Select `customer2-storage-network` in the Bridge Domain drop down
- Add `eda.nokia.com/role=edge` to the Interface Selector
- Set `2002` in the VLAN ID field

Click on <kbd>Add</kbd>, and then click on <kbd>Add To Transaction</kbd>. Notice your transactions basket in the top right now has one item in it, click on that icon to view the transaction basket contents.

To verify that our change will generate the configs we are looking for, from the transaction basket let's add a commit message click <kbd>Dry Run</kbd> to safely inspect the change scope.

You can now verify the diff in configuration between what was on the devices and what is about to be pushed! If you're happy with the change, go ahead and commit this transaction.

<iframe width="100%" height="600px" src="https://www.youtube.com/embed/IA06uDsQBT4" title="vnet storage 2002" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Step 2: Dealing with transaction failures

The happy flow was too easy. Let's see what happens when we encounter a failure in our transaction.

Navigate back to the `customer2` Virtual Network configuration form.  Add another VLAN with the following properties:

- VLAN Name: `customer2-storage-network-vlan-2`
- Select `customer2-storage-network` in the Bridge Domain drop down
- Add `eda.nokia.com/role=edge` to the Interface Selector
- Set `1000` in the VLAN ID field

To verify that our change will generate the configs we are looking for, let's add a commit message and hit <kbd>Dry Run</kbd> again to verify the change. Notice the transaction failed — head over to see the details of the transaction.

Looks like VLAN 1000 is already in use by `customer1`. We could go and fix our newly created VLAN to use a different VLAN, but we're going to make things more interesting by changing the VLAN in `customer1` from `1000` to `2000` while leaving `customer2`'s VLAN at 1000.

This will swap all existing subinterfaces using VLAN `1000` to VLAN `2000` and add new subinterfaces for VLAN `1000` in a new bridge domain... all in a single transaction!

Using the familiar workflow of editing the VLAN parameters, go to the `customer1` Virtual Network, change the VLAN ID, and add this change to the transaction basket. Dry-run it, and if all is OK, let's commit.

<iframe width="100%" height="600px" src="https://www.youtube.com/embed/ChWJ8ZmwjYc" title="VLAN swap" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Step 3 (Optional): Uh oh, didn't mean to do that

It's possible that although the devices accepted the change, the network may not be behaving as expected. Let's revert those changes to a time when life was good and the network was functioning as you wanted.

Head back to your terminal and let's take a look at the Git logs from all the successful transactions we've made so far.

```shell
edactl git log --reverse
```

Find a commit hash you want to revert to and issue the following command to restore back to that moment in time!

```shell
edactl git restore <hash>
```

You can go back to the UI or use some of the previously used K8s tooling to verify that the system has reverted back to when you expected!
