# Fedora Sway Setup
This will include all of my installed packages and why

### [RPM Fusion](https://docs.fedoraproject.org/en-US/quick-docs/rpmfusion-setup/)
This is needed for a couple of things but most importantly the WIFI!!!
### WiFi Drivers
Because the macbook uses broadcom we will need to install that after RPM Fusion. We have a couple of choices for to do this, but the basic one is
```
sudo dnf install broadcom-wl
```
### Graphics Driver
Check if Mesa is working
```
sudo dnf install mesa-demos
glxinfo | grep "OpenGL renderer"
```
Should see `Mesa Intel(R) Iris Pro`
### Power Management
TLP
```
sudo dnf install tlp tlp-rdw
sudo systemctl enable tlp --now
```
Powertop
```
sudo dnf install powertop
sudo powertop --auto-tune
```
### Keyboard Backlight
```
sudo dnf install brightnessctl
```
to test `brightness -l`
### Screen brightness
Add to our Sway Config
```
bindsym XF86MonBrightnessUp exec brightnessctl set +5%
bindsym XF86MonBrightnessDown exec brightnessctl set 5%-
```
Trackpad
```
sudo dnf install libinput
```
**Add to sway config**
```
input "type:touchpad" {
    tap enabled
    natural_scroll enabled
}
```
### Thermals
See Thermals
```
sudo dnf install lm_sensors
sudo sensors-detect
```
### Power Profiles Daemon
Set different power profiles
```
sudo dnf install power-profiles-daemon
```
## GPU Power Usage Fix
The iGPU can draw more power than needed
`sudo nano /etc/default/grub`
Find `GRUB_CMDLINE_LINUX=`
Add
```
i915.enable_psr=1 i915.enable_fbc=1
```
Run
```
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```
Reboot. This enables
- Panel Self Refresh
- Framebuffer Compression
- Lower idle power
### Wi-Fi Power Saving
```
iw dev wlan0 get power_save
```
If it says Off
```
sudo iw dev wlan0 set power_save on
```

## Sway Config Changes
Couple of things i changed in sway to make it more to my liking `nano ~/.config/sway/config`
### Monitor Scale 1.5 instead of 2
1. Check Display Output Name `swaymsg -t get_outputs`
2. Test Scaling `swaymsg output eDP-1 scale 1.5`
3. Add to Sway config:
```
output eDP-1 scale 1.5
```
### Set Background
Add to sway config
```
exec_always swaybg --image /path/to/image.jpg
```
