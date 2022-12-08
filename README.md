# Exam_2420
Name: Uday Chhina

Student ID: A01210638

Date: Dec 8, 2022

## Answers

### Part 1

```
$ sudo apt update && sudo apt upgrade
```

### Part 2

1. type `:set number` to enable line numbers
2. press `gg` to go to the first line
3. press `w` to go to the next word until the cursor is on `1`. Press `r` to
   replace the character with `0`
4. press `2gg` to move to second line. Press `l` to move to the character 'c'
   and press `a` to append 'h'. Press `esc` to exit insert mode. 
5.Press `e` to go to the next word's until
   the cursor is at `e` in `temparature`. Use `l` to move the cursor to `V`.
   Press `r` to replace `V` with `C`.
6. Press `5gg` to move to the 5th line. Press `r` to replace the `V` with `C`.
7. Press `10gg` to move to the 10th line. Press `w` until the cursor is on the
   `echo`, use `l` to move characcter to the character 'c'. Press `a` to
   append 'h'. Press `esc` to exit insert mode.
8. Press a combination of `w`s and `l`s until at the 'numbs' in the `sed`
   command. Press `R` to replace 'numbs' with ':digi'. Press `esc` to exit
   replacing and press i to insert the rest of the characters to complete
   ':digit:'. 
9, Press `11gg` and press `w` until the cursor is on the `echo`. Press `l` to
   move the cursor to the character 'c'. Press `a` to append 'h'. Press `esc`
   to exit insert mode.
10. You're done. Press `:wq` to save and exit.

![vimopen](/Images/openvim.png)

![vimedit](/Images/vimedit.png)

### Part 3

1. Open man page with `man journalctl`. 
2. Search with `/` for 'output': `/output` and press n to go to find `-o,
   --output=`. Use `json-pretty` as the output format.

![output](/Images/output.png)

3. Search for 'priority': `/priority` and press n to go to find `-p, it says
   that when a single value is specified it shows logs from lower bvalues
   meaning more important. 

![priority](/Images/priority.png)

4. Search for 'boot': `/boot` and press n to go to find `-b, --boot` which
   says that when nothing is specified it shows logs for the current boot. 

![boot](/Images/boot.png)

Unfortunately, there are no logs of warning or higher level for the current
boot.

![nologs](/Images/nologs.png)

5. The final command is:

![finalcommand](Images/finalcommand.png)


### Part 4

The script:

```bash
#!/bin/bash

USERS=$(grep ":[1-5][0-9][0-9][0-9]:" /etc/passwd | awk '{print $1}' FS=:)
IDS=$(grep ":[1-5][0-9][0-9][0-9]:" /etc/passwd | awk '{print $3}' FS=:)
SHELLS=$(grep ":[1-5][0-9][0-9][0-9]:" /etc/passwd | awk '{print $7}' FS=:)
LOGGEDIN=$(who | awk '{print $1}')


for (( i=0; i<${#USERS[@]}; i++ )); do
  LINES="${USERS[$i]} ${IDS[$i]} ${SHELLS[$i]}"
done

echo "${LINES}"

echo "Regular users on the system are:" > /etc/motd
for line in "$LINES"; do
  echo $line >> /etc/motd
done

echo " " >> /etc/motd
echo " " >> /etc/motd

echo "Users currently logged in are:" >> /etc/motd
echo $LOGGEDIN >> /etc/motd
```

### Part 5

The service file:
```
[Unit]
Description=find the current users

[Service]
Type=oneshot
ExecStart=/opt/find_users/find_users

[Install]
WantedBy=multi-user.target
```

The service file was placed in `/etc/systemd/system/find_users.service`.

The service was enabled with `sudo systemctl enable find_users.service`.

It was started with `sudo systemctl start find_users.service`.

The service was checked with `sudo systemctl status find_users.service`:

![service](/Images/service.png)

### Part 6

The timer file:
```
[Unit]
Description=timer to start the find users service on boot

[Timer]
OnBootSec=60
OnCalendar=*-*-* 00:00:00

[Install]
WantedBy=timers.target
```

The timer file was placed in `/etc/systemd/system/find_users.timer`.

The timer was enabled with `sudo systemctl enable find_users.timer`.

It was started with `sudo systemctl start find_users.timer`.

The timer was checked with `sudo systemctl status find_users.timer`:

![timer](/Images/timer.png)
