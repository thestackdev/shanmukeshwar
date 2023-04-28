---
title: "How to create swap area in linux"
date: 2023-04-23T18:21:57+05:30
draft: false
cover:
  image: "/blog/linux.jpeg"
  alt: "Linux"
  relative: false
---

If you're running Linux on a system with limited RAM, you may want to create a swap area to improve performance. A swap area allows your system to use disk space as virtual memory when your physical RAM is full. This can help prevent your system from crashing due to insufficient memory.

In this tutorial, we'll walk you through the steps to create a swap area in Linux.

## Step 1: Check if You Have a Swap Area

Before you create a new swap area, you should check if you already have one. To do this, run the following command in your terminal:

```
sudo swapon --show
```

If you see output that looks like this, you already have a swap area:

```
NAME TYPE SIZE USED PRIO
/dev/dm-1 partition 2G 0B -2
```

If you don't see any output, you don't have a swap area.

## Step 2: Create a Swap File

To create a new swap area, you can use a swap file. This is a regular file that resides on your filesystem and acts as a virtual memory space. To create a swap file, follow these steps:

1.  Determine the size of your swap file. As a general rule of thumb, your swap size should be at least equal to your physical RAM size, and no more than twice your physical RAM size. For example, if you have 4GB of RAM, you should create a swap file that's between 4GB and 8GB in size. In this tutorial, we'll create a 4GB swap file.
2.  Create the swap file using the `fallocate` command:

```
sudo fallocate -l 4G /swapfile
```

This command creates a 4GB file called `/swapfile`.

1.  Set the correct permissions on the file using `chmod`:

```
sudo chmod 600 /swapfile
```

This command sets the permissions of the swap file to read and write for the owner only.

## Step 3: Set Up the Swap Area

Now that you have a swap file, you need to set it up as a swap area. Follow these steps:

1.  Turn the file into a swap area using the `mkswap` command:

```
sudo mkswap /swapfile
```

This command initializes the swap file as a swap area.

1.  Activate the swap area using the `swapon` command:

```
sudo swapon /swapfile
```

This command activates the swap area so that it can be used by your system.

1.  Verify that the swap area is active by running the `swapon --show` command again:

```
sudo swapon --show
```

You should see output that looks like this:

```
NAME TYPE SIZE USED PRIO
/swapfile file 4G 0B -2
```

This indicates that your new swap area is active and ready to use.

## Step 4: Make the Swap Area Permanent

By default, your swap area will be lost when you reboot your system. To make the swap area permanent, you need to add an entry for it in your `/etc/fstab` file. Follow these steps:

1.  Open the `/etc/fstab` file in a text editor:

```
sudo nano /etc/fstab
```

1.  Add the following line to the end of the file:

```
/swapfile none swap sw 0 0
```

This line tells your system to use the file `/swapfile` as a swap area.

### Step 5: How to adjust the swappiness value

Swappiness is a Linux kernel property that defines how often the system will use the swap space. Swappiness can have a value between 0 and 100. A low value will make the kernel to try to avoid swapping whenever possible, while a higher value will make the kernel to use the swap space more aggressively.

```
echo 'vm.swappiness=10' >> /etc/sysctl.conf
```

The optimal swappiness value depends on your system workload and how the memory is being used. You should adjust this parameter in small increments to find an optimal value.
