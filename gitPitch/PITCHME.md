---

## Resource Quotas

---

### Introduction

Cloud Foundry offers quotas mechanism to manage available resources


  - disk 
  - memory
  - paid backing services

---

### Quota Structure

-	**name**: a name you will use to identify the plan;
-	**instance memory limit**: the maximum amount of memory that an instance can use, e.g., `256M` or `2G`
-	**memory limit**: the maximum amount of memory allowed to be used in the plan, e.g., `512M` or `1G`

---

### Quota Structure

-	**total routes**: the maximum number of routes allowed for the plan
-	**total services**: the maximum number of services allowed in the plan
-	**non basic services allowed**: setting this value to `true` will allow users to provision non-free service plans

---

### Listing quotas

Let's list the built-in plans:

```bash
cf quotas
```

As you can see, there is only one quota right now, which is the `default` quota.

---

### Getting quota info

It is also possible to get the same information in a more easy-to-read way:

```bash
cf quota default
```

---

### Creating and managing quotas

Creating a quota is very straightforward. Let's see the options:

```bash
cf help create-quota
```

**Reminder:** You can get help on a CLI command by doing `cf help command-name`.

---

### Creating and managing quotas

Now that we know what modifiers to use, we can create our first quota:

```bash
cf create-quota small-quota -i 512M -m 2048M -r 10 -s 5
```

* maximum instance memory (`-i`): 512M
* maximum memory allocation for all instances (`-m`): 2048M
* maximum number of routes (`-r`): 10
* maximum number of services (`-s`): 5 

---

### Creating and managing quotas

If you list the quotas again, you'll see that your newly created quota is there.

```bash
$ cf quotas
```

---

### Creating and managing quotas

Let's create a second quota, a larger one that allows paid plans:

```bash
cf create-quota large-quota -i 2048M -m 10G -r 100 -s 20 --allow-paid-service-plans
```

Listing the quotas again will show that `large-quota` has been created and is available.

---

### Modifying a quota

Modifying - or *updating* - a quota is very simple in Cloud Foundry. The format is almost the same as in the `create-quota` command.

As the first modification, let's disallow the use of paid plans in our `large-quota`:

```sh
cf update-quota large-quota --disallow-paid-service-plans
```

Now, users of this quota will not be able to provision any paid services.

---

### Modifying a quota

Also, we can modify any other quota parameter as we see fit:

```sh
cf update-quota small-quota -i 256M -s 2
```

As you can see, there is no need to re-specify all the parameters of a quota, except for the ones we need to modify.

---

### Assigning quotas

Now that we have created a quota, we can apply it to an org:

```sh
cf set-quota riman large-quota
```

---

### Assigning quotas

If you query `riman` details, you will see that the quota has been assigned to it:

```sh
cf org riman
```

**Caveat**: It is not possible to remove a quota from an org, once it has been assigned.

---

### Deleting a quota

Removing a quota definition from the system can be accomplished with:

```sh
cf delete-quota small-quota
```

**Reminder:** You can always use the `-f` modifier to force a command without confirmation.

---

### Space quotas

Space quotas are the same as regular quotas, but they can be assigned to specific spaces instead of orgs.

---

### Space quotas

Create a space quota with:

```sh
cf create-space-quota small-space-quota -i 512M -m 2048M -r 10 -s 5
```

---

### Space quotas

Now modify it:

```sh
cf update-space-quota small-space-quota -i 128M
```

---

### Space quotas

Assign the quota to your space:

```sh
cf set-space-quota hyper small-space-quota
```

---

### Space quotas

<span style="color:red">Now</span>, get the information for your space:

```sh
cf space hyper
```

As you can see, the space quota now appears in the space information, confirming that it was added successfully.

---

### Space quotas

The biggest difference between a space quota and an org quota is that a space quota can be unassigned:

```sh
cf unset-space-quota hyper small-space-quota
```

Before you complete this section, set the previously unset space quota back to the `hyper`.

---

### Space quotas

You can list all the space quotas or get information about a specific one, using the `space-quotas` and `space-quota` commands.

Try them!
