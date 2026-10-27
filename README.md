# asus_merlin_ssh_usage
Simple, small (1.36kb) function to add a usage commad. In SSH ash type "usage" &amp; a comprehensive list of interfaces &amp; usage metrics will return in the console log

I have tested in both `Firmware:386.12_4` on `RT-AC68U` as well as `Firmware:3006.102.7_2` on `GT-AX11000 Pro` & it functions the same on both.

Sample output
---

# GT-AX11000 Pro
```ash
User123@GT-AX11000_Pro-1234:/tmp/home/root# usage
Interface    Receive                     Transmit                    Total
-------------------------------------------------------------------------------
bcmsw        rx=8020317676910 (7.29 TiB) tx=8040695333394 (7.31 TiB) total=16061013010304 (14.61 TiB)
br0          rx=18047618256 (16.81 GiB)  tx=4829471716 (4.50 GiB)    total=22877089972 (21.31 GiB)
dummy0       rx=0                        tx=0                        total=0
erspan0      rx=0                        tx=0                        total=0
eth0         rx=0                        tx=0                        total=0
eth1         rx=97497510773 (90.80 GiB)  tx=147647975461 (137.51 GiB) total=245145486234 (228.31 GiB)
eth2         rx=0                        tx=0                        total=0
eth3         rx=4586546912452 (4.17 TiB) tx=3260410638490 (2.97 TiB) total=7846957550942 (7.14 TiB)
eth4         rx=3336027910355 (3.03 TiB) tx=4631505434604 (4.21 TiB) total=7967533344959 (7.25 TiB)
eth5         rx=245356244 (233.99 MiB)   tx=1131298189 (1.05 GiB)    total=1376654433 (1.28 GiB)
eth6         rx=963157 (940.58 KiB)      tx=15311678904 (14.26 GiB)  total=15312642061 (14.26 GiB)
eth7         rx=0                        tx=15310976370 (14.26 GiB)  total=15310976370 (14.26 GiB)
eth8         rx=0                        tx=15310952542 (14.26 GiB)  total=15310952542 (14.26 GiB)
gre0         rx=0                        tx=0                        total=0
gretap0      rx=0                        tx=0                        total=0
ifb0         rx=0                        tx=0                        total=0
ifb1         rx=0                        tx=0                        total=0
imq0         rx=0                        tx=0                        total=0
imq1         rx=0                        tx=0                        total=0
imq2         rx=0                        tx=0                        total=0
ip6_vti0     rx=0                        tx=0                        total=0
ip6gre0      rx=0                        tx=0                        total=0
ip6tnl0      rx=0                        tx=0                        total=0
ip_vti0      rx=0                        tx=0                        total=0
lo           rx=20630609439 (19.21 GiB)  tx=20630609439 (19.21 GiB)  total=41261218878 (38.43 GiB)
sit0         rx=0                        tx=0                        total=0
spu_ds_dummy rx=0                        tx=0                        total=0
spu_us_dummy rx=0                        tx=0                        total=0
```


# RT-AC68U
```ash
User123@@RT-AC68U-AB56:/tmp/home/root# usage
Interface    Receive                     Transmit                    Total
-------------------------------------------------------------------------------
br0          rx=196069734510 (182.60 GiB) tx=1021311173065 (951.17 GiB) total=1217380907575 (1.11 TiB)
dpsta        rx=0                        tx=0                        total=0
eth0         rx=2401544936 (2.24 GiB)    tx=3588728431 (3.34 GiB)    total=5990273367 (5.58 GiB)
eth1         rx=322380 (314.82 KiB)      tx=346762 (338.63 KiB)      total=669142 (653.46 KiB)
eth2         rx=0                        tx=0                        total=0
ifb0         rx=0                        tx=0                        total=0
ifb1         rx=0                        tx=0                        total=0
lo           rx=1078533659 (1.00 GiB)    tx=1078533659 (1.00 GiB)    total=2157067318 (2.01 GiB)
vlan1        rx=193146308931 (179.88 GiB) tx=1018903392556 (948.93 GiB) total=1212049701487 (1.10 TiB)
vlan2        rx=4506264 (4.30 MiB)       tx=35695254 (34.04 MiB)     total=40201518 (38.34 MiB)
vlan3        rx=1004897260144 (935.88 GiB) tx=195830454529 (182.38 GiB) total=1200727714673 (1.09 TiB)
wl0.2        rx=812275270 (774.65 MiB)   tx=1567894885 (1.46 GiB)    total=2380170155 (2.22 GiB)
```


Installation is simple. the script will be written to the JFFS partition.

1. SSH into the Router. You will be connected into a busybox ASH shell
2. The following commands will be run:
    1. `mkdir -p /jffs/configs` | this will make a `configs` directory on the jffs mount for the script to live in
    2. `nano /jffs/configs/profile.add` | This will create a new file, or edit a file if it already exists, named `profile.add` in Nano which is already included I belive in Merlin. You can use vim if you prefer which I believe is also installed. Copy the text in [profile.add](profile.add) & paste it into nano. To exit nano `[Ctrl]`+`X` then type `y` to save the file or overwrite the old file, then hit `[Enter]`
    3. `. /jffs/configs/profile.add` | starting with the `.` this will load the script as a function & enable it. I believe the script will automatically load when the profile starts, like when you connect SSH, but if it doesn't running this command will add it
    4. `usage` | This will run the function as shown above.  This will note the usage of each interface & the names of each interface. For the `RT-AC68U` `vlan2` is the USB-Tethered modem, & `vlan3` is a failover 2nd WAN set to a LAN port. In my case LAN1, but I believe it is the same no matter what port is used. The `GT-AX11000 Pro` does not show vlan, but that is likely due to it not being used for WAN traffic & being a mid-network device in the current setup. The spacing is designed to work with a 120 wide console diplay without multi-line overflow but could be adjusted to fit your needs.
