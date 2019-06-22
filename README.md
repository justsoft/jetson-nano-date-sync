# jetson-nano-date-sync
Automatically synchronize Jetson Nano's system time.

You might experince same issue as what I faced, the Jetson Nano wouldn't be able to synchronize it's time with NTP server.
When I manually update the system time by following command, I got: `no server suitable for synchronization found`
```
sudo ntpdate -dv ntp.ubuntu.com
```
It turns out I have to find a workaround. This way doesn't have the accuracy as NTP, but a fast alternative.

## Install
1. Download or simple pipe the date-sync file to `/etc/network/if-up.d/` folder.
> ```sudo wget -O /etc/network/if-up.d/date-sync https://raw.githubusercontent.com/justsoft/jetson-nano-date-sync/master/date-sync```
2. Change (optional) the URL `http://bing.com` to a fast and reliable one
> ```sudo vi /etc/network/if-up.d/date-sync```
3. Add excutable attribute to the `/etc/network/if-up.d/date-sync`:
> ```sudo chmod +x /etc/network/if-up.d/date-sync```

# Raspbian
If raspbian can not synchronize it's clock with internet time server, this workaround can help you. Let's check how the timedatectl tells:
```
pi@raspberrypi:~ $ timedatectl status
      Local time: Mon 2019-04-08 11:03:38 MDT
  Universal time: Mon 2019-04-08 17:03:38 UTC
        RTC time: n/a
       Time zone: America/Edmonton (MDT, -0600)
 Network time on: yes
NTP synchronized: no
 RTC in local TZ: no
```
Check the timesyncd.service status:
```
pi@raspberrypi:~ $ systemctl status systemd-timesyncd.service
● systemd-timesyncd.service - Network Time Synchronization
   Loaded: loaded (/lib/systemd/system/systemd-timesyncd.service; enabled; vendor preset: enabled)
  Drop-In: /lib/systemd/system/systemd-timesyncd.service.d
           └─disable-with-time-daemon.conf
   Active: active (running) since Mon 2019-04-08 10:48:18 MDT; 2min 46s ago
     Docs: man:systemd-timesyncd.service(8)
 Main PID: 268 (systemd-timesyn)
   Status: "Connecting to time server 193.228.143.22:123 (0.debian.pool.ntp.org)."
   CGroup: /system.slice/systemd-timesyncd.service
           └─268 /lib/systemd/systemd-timesyncd

Apr 08 10:48:18 raspberrypi systemd[1]: Started Network Time Synchronization.
Apr 08 10:49:02 raspberrypi systemd-timesyncd[268]: Timed out waiting for reply from 139.199.215.251:123 (2.debian.pool.ntp.org).
Apr 08 10:49:12 raspberrypi systemd-timesyncd[268]: Timed out waiting for reply from 185.209.85.222:123 (2.debian.pool.ntp.org).
Apr 08 10:49:22 raspberrypi systemd-timesyncd[268]: Timed out waiting for reply from 78.46.102.180:123 (2.debian.pool.ntp.org).
Apr 08 10:49:33 raspberrypi systemd-timesyncd[268]: Timed out waiting for reply from 119.28.206.193:123 (2.debian.pool.ntp.org).
Apr 08 10:49:43 raspberrypi systemd-timesyncd[268]: Timed out waiting for reply from 185.255.55.20:123 (3.debian.pool.ntp.org).
Apr 08 10:49:53 raspberrypi systemd-timesyncd[268]: Timed out waiting for reply from 144.76.76.107:123 (3.debian.pool.ntp.org).
Apr 08 10:50:03 raspberrypi systemd-timesyncd[268]: Timed out waiting for reply from 45.43.30.59:123 (3.debian.pool.ntp.org).
Apr 08 10:50:14 raspberrypi systemd-timesyncd[268]: Timed out waiting for reply from 94.130.49.186:123 (3.debian.pool.ntp.org).
Apr 08 10:50:56 raspberrypi systemd-timesyncd[268]: Timed out waiting for reply from 193.228.143.12:123 (0.debian.pool.ntp.org)
```
So, the raspbian's default time synchronization doesn't work here, we need this workaround.

## Install for Raspberry Pi (Raspbian)
1. Download or simple pipe the date-sync.service file to `/etc/systemd/system/` folder.
> ```sudo wget -O /etc/systemd/system/date-sync.service https://raw.githubusercontent.com/justsoft/jetson-nano-date-sync/master/date-sync.service```
2. Change (optional) the URL `http://bing.com` to a fast and reliable web server
> ```sudo vi /etc/systemd/system/date-sync.service```
3. Check the date-sync.service systemd status:
> ```sudo systemctl status date-sync.service ```
4. Enable the date-sync.service:
> ```sudo systemctl enable date-sync.service```
5. Reboot
