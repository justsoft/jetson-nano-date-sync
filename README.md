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
2. Change the URL `http://bing.com` to a fast and reliable one
> ```sudo vi /etc/network/if-up.d/date-sync```
3. Add excutable attribute to the `/etc/network/if-up.d/date-sync`:
> ```sudo chmod +x /etc/network/if-up.d/date-sync```
