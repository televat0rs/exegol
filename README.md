# EXEGOL
## burp setup
**binding these folders persists your extensions/configs/updates and fixes licensing/permissions**

**if burp browser doesn't open: Settings>Tools>Burp's browser>Clear all**
#
`mkdir /root/.exegol/my-resources/setup/BurpSuitePro`

`mkdir /root/.exegol/my-resources/setup/.BurpSuite`

`mkdir /root/.exegol/my-resources/setup/.java`

`mount --bind /home/kali/BurpSuitePro /root/.exegol/my-resources/setup/BurpSuitePro`

`mount --bind /home/kali/.BurpSuite /root/.exegol/my-resources/setup/.BurpSuite`

`mount --bind /home/kali/.java /root/.exegol/my-resources/setup/.java`

## add to /etc/fstab for persistence after testing the mounts

`/home/kali/BurpSuitePro /root/.exegol/my-resources/setup/BurpSuitePro none bind 0 0`

`/home/kali/.BurpSuite /root/.exegol/my-resources/setup/.BurpSuite none bind 0 0`

`/home/kali/.java /root/.exegol/my-resources/setup/.java none bind 0 0`

## copy stuff to .exegol/my-resources

`cp .tmux.conf /root/.exegol/my-resources/setup/tmux/tmux.conf`

`cp ctf.sh /root/.exegol/my-resources/bin/ctf.sh`

`cp load_user_setup.sh /root/.exegol/my-resources/setup/load_user_setup.sh`

#

### KRB_AP_ERR_SKEW(Clock skew too great)
#### ntpdate doesn't work currently, but faketime might if the DC isn't too sensitive

`output=$(getST.py -spn cifs/dc.freelancer.htb -impersonate administrator 'freelancer.htb/hello$:password' -debug 2>&1); echo "$output"; server_time=$(echo "$output" | grep -oP 'Server time \(UTC\): \K.*' | head -n 1); echo "Extracted Server Time: $server_time"; if [[ -n "$server_time" ]]; then faketime "$server_time" zsh -c "date; exec zsh"; else echo "Server time not found in output."; fi`

#
#
#

# CTF

### copy the '.md' to your ctf folder

- start a terminal (Alacritty) and IDE (VSCode)
- edit the vpn path and CTF= value and exec
- replace the TARGET= with the challenge ip and exec
#
### start container
```sh
CTF=permx.htb; mkdir -p /home/kali/ctf/$CTF ; cd /home/kali/ctf/$CTF ; mv "/home/kali/ctf/.md copy" "/home/kali/ctf/$CTF/$CTF.md" ; exegol start $CTF nightly -fs -cwd -s zsh --vpn /home/kali/ctf/htb_s6.ovpn
```

**replacing the CTF= value with a similar "fqdn type" (ctf.htb, ctf.vl, etc.) enables some automation through ctf.sh**

*challenges don't always use this hostname, but it shouldn't break anything*

#
### start environment
```sh
export TARGET=10.10.11.23; export DOMAIN=$(echo $HOSTNAME | sed 's/^exegol-//'); export ATTACKER_IP=$(ip addr show tun0 | grep 'inet ' | awk '{print $2}' | cut -d/ -f1); /opt/my-resources/bin/ctf.sh
```

**ctf.sh sets up the environment and exports 3 handy variables into the tmux session**

> $TARGET

> $ATTACKER_IP

> $DOMAIN
