# Day 7: Linux SSH Authentication
We use password-less authentication via SSH keys to securely and automatically connect to servers without exposing passwords in scripts.


# Table of Contents
+ [The Task:](#The-Task)
+ [Goal:](#Goal)
+ [Manual Solution:](#Manual-Solution:)
+ [Shell Script Solution:](#Shell-Script-Solution) 

--------------------------------------------------------------------
### The Task:

The system admins team of xFusionCorp Industries has set up automation scripts on the jump host that need to run on all app servers in the Stratos Datacenter.

To enable these scripts to work properly, the thor user on the jump host must have password-less SSH access to all app servers using their respective sudo users:

App Server 1 → tony

App Server 2 → steve

App Server 3 → banner

--------------------------------------------------------------------

### Goal:

Set up SSH key-based authentication from the thor user on the jump host to each app server, so scripts can execute remotely without requiring passwords.

--------------------------------------------------------------------

### Manual Solution:

Step 1: Generate SSH key for thor on jump host:

Log in as thor on the jump host:

ssh-keygen -t ed25519 -C "thor-jumphost-to-app-servers"
or ssh-keygen -t rsa


Press Enter to accept default location (~/.ssh/id_ed25519)

Leave passphrase empty for full automation

This creates:

Private key: ~/.ssh/id_ed25519

Public key: ~/.ssh/id_ed25519.pub

Step 2: Copy the public key to each app server

Use the respective sudo users to copy the key:

ssh-copy-id tony@1.1.1.1
ssh-copy-id steve@2.2.2.2
ssh-copy-id banner@3.3.3.3


Enter the password for each user when prompted (one time).

This adds thor’s public key to each server’s ~/.ssh/authorized_keys.

✅ After this, thor can SSH into each server without a password.

Step 3: Test password-less SSH
ssh tony@172.16.238.10
ssh steve@172.16.238.11
ssh banner@172.16.238.12


You should be logged in without entering a password.
