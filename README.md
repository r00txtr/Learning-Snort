# How to Install and Configure Snort (IDS/IPS) on Ubuntu: A Beginner-Friendly Guide

Snort is a powerful and widely-used open-source Intrusion Detection System (IDS) and Intrusion Prevention System (IPS) that monitors network traffic for suspicious activity. In this guide, we'll walk you through the steps to install and configure Snort on an Ubuntu system. By the end of this tutorial, you'll have Snort up and running, ready to detect potential threats on your network.

### Prerequisites
Before we begin, make sure you have:
- A system running Ubuntu (this guide uses Ubuntu 20.04 LTS).
- Basic knowledge of the Linux command line.
- Root or sudo access on your system.

### Step 1: Install Snort

The first step is to install Snort on your Ubuntu machine. Open a terminal and run the following command:

```bash
sudo apt-get install snort -y
```

This command will download and install Snort and its dependencies.

### Step 2: Verify the Installation

After the installation is complete, verify that Snort is installed correctly by checking its version:

```bash
snort --version
```

You should see an output similar to the following, which confirms that Snort is installed:

```bash
   ,,_     -*> Snort! <*-
  o"  )~   Version 2.9.17.1 GRE (Build 149) 
   ''''    By Martin Roesch & The Snort Team
           http://www.snort.org/contact#team
           Copyright (C) 1998-2020 Cisco and/or its affiliates. All rights reserved.
```

### Step 3: Identify Your Network Interface

Next, you'll need to identify the network interface that Snort will monitor. You can list all network interfaces on your system using the `ifconfig` command:

```bash
ifconfig
```

Look for the interface that you want Snort to monitor (e.g., `enp0s8`). Make a note of this interface name, as you'll need it in the next steps.

### Step 4: Enable Promiscuous Mode

For Snort to capture all network traffic on the interface, it must be set to promiscuous mode. Use the following command to enable promiscuous mode on your chosen interface (`enp0s8` in this example):

```bash
sudo ip link set enp0s8 promisc on
```

You can verify that promiscuous mode is enabled by running:

```bash
ifconfig enp0s8
```

You should see `PROMISC` listed in the output, indicating that promiscuous mode is active.

### Step 5: Backup the Snort Configuration File

Before making any changes, it's always a good idea to create a backup of the original Snort configuration file:

```bash
sudo cp /etc/snort/snort.conf /etc/snort/snort.conf.backup
```

This backup allows you to restore the original settings if something goes wrong during configuration.

### Step 6: Configure Snort

Now, let's edit the Snort configuration file to suit your needs. Open the configuration file in a text editor:

```bash
sudo nano /etc/snort/snort.conf
```

In the configuration file, you may need to modify the following sections:

1. **Network Variables**: Define your network variables like `HOME_NET` to specify which network Snort should monitor. For example, if your network IP range is `192.168.1.0/24`, update the `ipvar HOME_NET` line as follows:

    ```bash
    ipvar HOME_NET 192.168.1.0/24
    ```

2. **Rule Paths**: Ensure that the paths for your rule files are correct. By default, Snort looks for rules in `/etc/snort/rules/`. You can download and update rulesets from [Snort’s official website](https://www.snort.org/downloads).

After making your changes, save the file by pressing `CTRL + O`, and then exit the editor with `CTRL + X`.

### Step 7: Test Your Configuration

Before starting Snort, it’s important to test the configuration to ensure there are no errors. Run the following command to test the configuration file:

```bash
sudo snort -T -i enp0s8 -c /etc/snort/snort.conf
```

Here’s what each option means:
- `-T`: Test the configuration and exit.
- `-i enp0s8`: Specify the network interface to monitor.
- `-c /etc/snort/snort.conf`: Specify the path to the Snort configuration file.

If everything is configured correctly, you should see an output indicating that the test was successful.

### Step 8: Run Snort

Now that your configuration is tested and working, you can start Snort to monitor your network:

```bash
sudo snort -i enp0s8 -c /etc/snort/snort.conf
```

Snort will begin monitoring the specified interface for any suspicious activity based on the rules defined in your configuration file.

### Conclusion

Congratulations! You've successfully installed and configured Snort on your Ubuntu system. Snort is now actively monitoring your network traffic and will alert you to any potential threats. This guide covered the basics, but there's much more to explore with Snort, such as writing custom rules and integrating it with other security tools. Keep experimenting and learning to strengthen your network defense skills.

If you found this guide helpful, consider sharing it with others and check out more tutorials on defensive security topics. Happy monitoring!
