# Useful Commands for Creating New Linux Account

```bash
# Create a new linux account from admin
sudo useradd -m <username>

# Create user password
sudo passwd <username>

# Change shell for the new user
sudo chsh -s /bin/bash <username>

###
# Add the user to sudoers (Optional)
# Step 1) open the file at /etc/sudoers with visudo
# Step 2) append the new username
###

# Step 1
sudo visudo

# Step 2
<username>       ALL=(ALL:ALL) ALL
```
