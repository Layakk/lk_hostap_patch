The following two patches provide extra functionality to hostap:
---
patch_layakk_hostap-MACs_Anonymization-a60dbbce443d62c403492549a1353ed1a3eaefb4
patch_layakk_hostap-Vendor_ACLs-a60dbbce443d62c403492549a1353ed1a3eaefb4
---

They were last tested against the following git commit of hostap:
a60dbbce443d62c403492549a1353ed1a3eaefb4

How to apply them:
NOTE: The order is important, since one file is modified by both patches.
---
dl:tmp root # git clone git://w1.fi/srv/git/hostap.git
Cloning into 'hostap'...
remote: Counting objects: 56497, done.
remote: Compressing objects: 100% (11058/11058), done.
remote: Total 56497 (delta 46170), reused 55367 (delta 45269)
Receiving objects: 100% (56497/56497), 12.23 MiB | 2.90 MiB/s, done.
Resolving deltas: 100% (46170/46170), done.
dl:tmp root # cd hostap
dl:hostap root [master] # git checkout a60dbbce443d62c403492549a1353ed1a3eaefb4
dl:hostap root [master] # git log --pretty=oneline -3
a60dbbce443d62c403492549a1353ed1a3eaefb4 tests: ANQP-QUERY-DONE event
fb09ed338919db09f3990196171fa73b37e7a17f Interworking: Notify the ANQP parsing status
d10b01d299bdded9552b2c89cada963c991b5d1c HS20: Provide appropriate permission to the OSU related files
dl:hostap root [master] # ls -la ../patch_layakk_hostap-*
-rw-r--r-- 1 root root   932 Jan 20 17:58 ../patch_layakk_hostap-MACs_Anonymization-a60dbbce443d62c403492549a1353ed1a3eaefb4
-rw-r--r-- 1 root root 22338 Jan 20 08:57 ../patch_layakk_hostap-Vendor_ACLs-a60dbbce443d62c403492549a1353ed1a3eaefb4
dl:hostap root [master] # patch -p1 < ../patch_layakk_hostap-Vendor_ACLs-a60dbbce443d62c403492549a1353ed1a3eaefb4
patching file hostapd/config_file.c
patching file hostapd/ctrl_iface.c
patching file hostapd/hostapd.conf
patching file src/ap/ap_config.c
patching file src/ap/ap_config.h
patching file src/ap/beacon.c
patching file src/ap/ieee802_11.c
patching file src/ap/ieee802_11_auth.c
patching file src/ap/ieee802_11_auth.h
patching file src/utils/common.c
patching file src/utils/common.h
dl:hostap root [master] # patch -p1 < ../patch_layakk_hostap-MACs_Anonymization-a60dbbce443d62c403492549a1353ed1a3eaefb4
patching file hostapd/Makefile
patching file src/utils/common.h
dl:hostap root [master] # find . -name "*.orig"
dl:hostap root [master] # find . -name "*.rej"
dl:hostap root [master] # 
---


Once the patches have been applied, if you want to anonymize the MACs, you must uncomment the corresponding line in file Makefile:
---
dl:hostap root [master] # cd hostapd
dl:hostapd root [master] # vi Makefile
dl:hostapd root [master] # grep ANON Makefile
#CFLAGS += -DANONYMIZE_MACADDRS
dl:hostapd root [master] #
---

Finally, you can compile hostapd as usual:
---
dl:hostapd root [master] # cp defconfig .config
dl:hostapd root [master] # make
---
