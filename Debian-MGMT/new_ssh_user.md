sudo adduser louc --disabled-password
sudo deluser --remove-all-files yann-go

mkdir .ssh
cd .ssh
ssh-keygen -m PEM -t rsa -b 4096 -C "test@test.test"
cat filename.pub >> authorized_keys

sudo nano /etc/ssh/sshd_config

Add the following to the last.

Match User username
  PasswordAuthentication no
  AllowTcpForwarding yes
  X11Forwarding no
  AllowAgentForwarding no