---
hide: navigation
---

# Lab 2: It's Friday... Go Ahead, Push to Prod

Need to add a VLAN to your data center? Push some new ACLs or BGP policies on your border leafs? Best not attempt that on a Friday in case something goes wrong. But what if it were safe to do so? What if you knew exactly what was going to happen before making the change and knew that if anything were to go wrong the entirety of your network would be rolled back to a good working state?

Enter EDA, your network change savior, the ambassador of stress free weekends.

In EDA every configuration change is done in an all-or-nothing fashion and transacted in Git. Always.

If the desired state is not achievable even on a single device, the whole transaction is pronounced failed and the changes are reverted immediately from all of the nodes.

The transactions are network-wide.

When you create any resource, EDA automatically initiates a transaction and publishes its result.

Transactions are one of the key components to ensure we reduce the human error to 0, lets dig in a bit deeper.

## Step 1: What's the Diff?

Log into the EDA UI by visiting the following:

| Connection | URL/Command                             | Example                      | Credentials                 |
| ---------- | --------------------------------------- | ---------------------------- | --------------------------- |
| Web        | `https://nfd`**`<id>`**`.srexperts.net` | <https://nfd1.srexperts.net> | `admin`<br/>`nfd+eda@nokia` |

- Navigate to the Fabrics in the left-hand menu; you should see the fabric we created in Lab 1.
- Double-click on the fabric object in the table to open up a configuration form.

From here, let's go ahead and change our Interswitch Links from using assigned IPv6 Global addresses to Unnumbered IPv6 LLA.

- You can use the filter in the top left of the form to help find the fields you are looking for. You can start typing `Interswitch`, for example. Once you've found the correct fields, let's clear the IPv6 pool and use Unnumbered IPv6.

Go ahead and add this change to a transaction.

Notice your transactions basket in the top right now has one item in it.

To verify that our change will generate the configs we are looking for, let's add a commit message and dry-run the change.

You can now verify the diff in configuration between what was on the devices and what is about to be pushed! If you're happy with the change, go ahead and commit this transaction.

<iframe width="1236" height="1000" src="https://www.youtube.com/embed/fVjviLr_pYA" title="BGP unnumbered" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Step 2: Fail

The happy flow was too easy. Let's see what happens when we encounter a failure in our transaction.

Navigate back to the fabric form and let's reintroduce IPv4 on our ISLs. This time, we're going to use an IPv4 address pool from which to allocate addresses. Select the `pool-new-ipv4` allocation pool, and let's add this change to our transaction and dry-run it again.

Notice the transaction failed, head over to see the details of the transaction.

Now, let's fix the issue by increasing the size of our IPv4 subnet allocation pool from the errored /30 to a /16, which should allow enough addresses to be assigned.

Add the change to the transaction basket. There are now two changes to be made at once—dry-run it, and if complete, go ahead and commit!

<iframe width="1221" height="986" src="https://www.youtube.com/embed/kNwbCuM2GYM" title="failed transaction pool" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Step 3 (Optional): Uh oh, didn't mean to do that

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

<iframe width="1221" height="990" src="https://www.youtube.com/embed/TqfE0Kki08M" title="fabric restore" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
