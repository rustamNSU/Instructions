# Connect to remote server via ssh connection
1. install openssh (or putty for windows)

2. add (or create) sscc configuration into ~/.ssh/config
```bash
Host sscc_abdullin
HostName nks-1p.sscc.ru
IdentityFile /Users/rustam/.ssh/sscc_abdullin
User abdullin_r_f
StrictHostKeyChecking no
AddKeysToAgent yes
ServerAliveInterval 120
```
where Host -- title for convenience, IdentityFile -- path to private key (in openssh format, rsa 3072)

3. Run session via command
```bash
ssh sscc_abdullin
```

4. If you can't connect to remote server with the error ending as "Please make sure you have the correct access rights and the repository exists." (when coping private key from telegram or from pc to server) change access mode of private key
```bash
chmod 400 ~/.ssh/<private key>
```

# ssh-connection for self-hosted gitlab (gpn)
1. Add ssh key-pair in gitlab account ```Profile > Preferences > SSH Keys```. Add existing pair or generate new like
```bash
ssh-keygen  -t rsa -b 3072 -f gpn-gitlab-host-key
```
2. add gitlab ssh configuration into ~/.ssh/config (if an access mod error is occurred, change access mod in terminal, see 4 in previous section)
```bash
Host gitlab-gpn-noc.nsu.ru
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/gpn-gitlab-host-key
```

3. clone repository using ssh ```git clone <ssh url>``` eg
```bash
git clone git@gitlab-gpn-noc.nsu.ru:hydrofracteam/fracturing_simulators/poroelasticity/poroelastic_frac_simulator_2d.git