---
hide: navigation
---

# Lab 2: It's Friday... Go Ahead, Push to Prod!

Need to add a VLAN to your data center? Push some new ACLs or BGP policies on your border leafs? Best not attempt that on a Friday in case something goes wrong. But what if it were safe to do so? What if you knew exactly what was going to happen before making the change and knew that if anything were to go wrong the entirety of your network would be rolled back to a good working state?

Enter EDA, your network change savior, the ambassador of stress free weekends.

In EDA every configuration change is done in an all-or-nothing fashion and transacted in Git. Always.

If the desired state is not achievable even on a single device, the whole transaction is pronounced failed and the changes are reverted immediately from all of the nodes.

The transactions are network-wide.

When you create any resource, EDA automatically initiates a transaction and publishes its result.

Transactions are one of the key components to ensure we reduce the human error to 0, lets dig in a bit deeper.

## Step 1: VLAN Creation using transactions, what's the Diff?

Log into the EDA UI by visiting the following:

| Connection | URL/Command                       | Example                |
| ---------- | --------------------------------- | ---------------------- |
| Web        | `https://nfd`**`<id>`**`.eda.dev` | <https://nfd1.eda.dev> |


- Navigate to the Virtual Networks in the left-hand menu.
- Double-click on the Virtual Network called `customer2` object in the table to open up a configuration form.

From here, let's add a VLAN to our storage network.  

- You can use the filter in the top left of the form to help find the fields you are looking for. You can start typing `vlan`, for example. Once you've found the correct VLAN table, click on `Add` 

Fill out the form as follows:

- VLAN Name: `customer2-storage-network-vlan-1`
- Select `customer2-storage-network` in the Bridge Domain drop down
- Add `eda.nokia.com/role=edge` to the Interface Selector
- Set `2002` in the VLAN ID field

Click on `Add`, and then click on `Add To Transaction`. Notice your transactions basket in the top right now has one item in it.

To verify that our change will generate the configs we are looking for, let's add a commit message and dry-run the change.

You can now verify the diff in configuration between what was on the devices and what is about to be pushed! If you're happy with the change, go ahead and commit this transaction.

<iframe width="1222" height="989" src="https://www.youtube.com/embed/IA06uDsQBT4" title="vnet storage 2002" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Step 2: Dealing with transaction failures

The happy flow was too easy. Let's see what happens when we encounter a failure in our transaction.

Navigate back to the `customer2` Virtual Network configuration form.  Add another VLAN with the following properties:

- VLAN Name: `customer2-storage-network-vlan-2`
- Select `customer2-storage-network` in the Bridge Domain drop down
- Add `eda.nokia.com/role=edge` to the Interface Selector
- Set `1000` in the VLAN ID field

To verify that our change will generate the configs we are looking for, let's add a commit message and dry-run the change.  Notice the transaction failed, head over to see the details of the transaction.

Looks like VLAN 1000 is already in use by `customer1`, lets fix our typo by heading back to our transaction basket and click on the pencil indicating we want to edit the Virtual Network in our transaction basket. 

Edit our newly created VLAN to use VLAN ID 2000 instead of 1000.

Add the change to the transaction basket, dry-run it and if all OK lets commit.

<iframe width="1222" height="986" src="https://www.youtube.com/embed/3W-xbt-Prng" title="vnet storage 1000" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Step 3 (Optional): Uh oh, didn't mean to do that...

It's possible that although the devices accepted the change, the network may not be behaving as expected. Let's revert those changes to a time when life was good and the network was functioning as you wanted.

Head back to your terminal and let's take a look at the Git logs from all the successful transactions we've made so far.

```
edactl git log --reverse
```

Find a commit hash you want to revert to and issue the following command to restore back to that moment in time!

```
 edactl git restore <hash>
```

You can go back to the UI or use some of the previously used K8s tooling to verify that the system has reverted back to when you expected!
