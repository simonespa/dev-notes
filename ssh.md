# SSH

- [Secure copy with SSH](#secure-copy-with-ssh)
- [Starting ssh-add](#starting-ssh-add)
- [Generate SSH keys](#generate-ssh-keys)
- [SSH Tunnel](#ssh-tunnel)
- [Config example](#config-example)
- [References](#references)
- [Concetps](#concetps)

## Secure copy with SSH

```
scp ./script.js user@server:/home/simone_spaccarotella/script.js
```

By default scp uses the Triple-DES cipher to encrypt the data being sent. Using the Blowfish cipher has been shown to increase speed. This can be done by using option `-c blowfish` in the command line.

```
scp -c blowfish
```

## Starting ssh-add

```
exec /usr/bin/ssh-agent $SHELL
ssh-add
```

## Generate SSH keys

From https://help.github.com/articles/generating-ssh-keys/

```
ssh-keygen -t rsa -b 4096 -C "spa.simone@gmail.com"
ssh-keygen -o -a 100 -t rsa -b 4096 -C "spa.simone@gmail.com"
```

## SSH Tunnel

```
ssh -f user@server -L 6379:tes-ca-1m7i83q2963sy.agjegn.0001.euw1.cache.amazonaws.com:6379 -N
redis-cli -h 127.0.0.1 -p 6379
```

## Config example

**Example 1**

```
Host sandbox7
  HostName cloud7.sandbox.bbc.co.uk
  User developer

Host *
  AddKeysToAgent yes
  UseKeychain yes
```

**Example 2**

```
Host bastion
  HostName 52.28.226.209
  Port 22
  User ec2-user
  IdentityFile ~/.ssh/torneo.key

Host webserver
  HostName 10.100.7.62
  Port 22
  User ec2-user
  IdentityFile ~/.ssh/torneo.key
  ProxyCommand ssh ec2-user@52.28.226.209 nc %h %p

Host nat
  HostName 10.100.7.67
  Port 22
  User ec2-user
  IdentityFile ~/.ssh/torneo.key
  ProxyCommand ssh ec2-user@52.28.226.209 nc %h %p
```

## References

- http://linux.die.net/man/5/ssh_config
- http://www.cyberciti.biz/faq/create-ssh-config-file-on-linux-unix/

## Concetps

- SSH Bastion Host
- ForwardAgent vs ProxyCommand
