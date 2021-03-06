#$L$
# Copyright (C) 2014 Ridgerun (http://www.ridgerun.com). 
##$L$

# We don't have to fetch the source code, is local
FETCHER_NO_DOWNLOAD=yes
INTERFACES=$(DEVDIR)/myapps/rw-partition/extras_overlay/rw-partition/etc/network/interfaces
RW_PARTITION=$(DEVDIR)/myapps/rw-partition/extras_overlay/rw-partition/

include ../../bsp/classes/rrsdk.class

build:
ifeq ($(CONFIG_BSP_NET_HAS_NET),y)
	$(V)mkdir -p $(RW_PARTITION)/etc/network/ $(RW_PARTITION)/var/run/ $(QOUT)
	$(V)ln -sf /var/run $(RW_PARTITION)/etc/network/
	$(V)echo -e "# Warning this file is automatically generated, you can overwrite it with a file from the target file system overlay" > $(INTERFACES)
	$(V)echo -e "\n# The loopback network interface" >> $(INTERFACES)
	$(V)echo -e "auto lo\niface lo inet loopback\n" >> $(INTERFACES)

ifeq ($(CONFIG_BSP_NET_HAS_ETH0),y)
	$(V)echo -e "auto eth0" >> $(INTERFACES)
ifeq ($(CONFIG_FS_TARGET_NFSROOT),y)
	$(V)echo "#SDK configured for NFS root, so we don't mess with eth0 ipv4" >> $(INTERFACES)
	$(V)echo "iface eth0 inet manual" >> $(INTERFACES)
else
ifneq ($(CONFIG_BSP_NET_ETH0_USE_DHCP),y)
	$(V)echo "iface eth0 inet static" >> $(INTERFACES)
	$(V)echo "	address $(CONFIG_BSP_NET_ETH0_IPADDR)" >>$(INTERFACES)
	$(V)echo "	netmask $(CONFIG_BSP_NET_ETH0_NETMASK)" >>$(INTERFACES)
else
	$(V)echo "iface eth0 inet dhcp" >> $(INTERFACES)
ifeq ($(CONFIG_UDHCPC),y)
	$(V)echo "	script /etc/udhcp.script" >> $(INTERFACES)
endif
endif #DHCP
endif #NFSROOT
ifeq ($(CONFIG_BSP_IPV6_SUPPORT),y)
ifeq ($(CONFIG_BSP_NET_ETH0_USE_DHCPV6),y)
	$(V)echo "iface eth0 inet6 dhcp" >> $(INTERFACES)
endif # ETH0_DHCPV6
ifeq ($(CONFIG_BSP_NET_ETH0_USE_STATIC6),y)
	$(V)echo "iface eth0 inet6 static" >> $(INTERFACES)
	$(V)echo "	address $(CONFIG_BSP_NET_ETH0_GLOBAL_IPADDR6)/64" >>$(INTERFACES)
	$(V)echo "	gateway $(CONFIG_BSP_NET_ETH0_GATEWAY_IPADDR6)" >>$(INTERFACES)
endif # ETH0_STATIC6
ifeq ($(CONFIG_BSP_NET_ETH0_USE_STATELESS),y)
	$(V)echo "iface eth0 inet6 manual" >> $(INTERFACES)
endif # ETH0_STATELESS
endif # IPV6_SUPPORT
endif # ETH0

ifeq ($(CONFIG_BSP_NET_HAS_USB0),y)
	$(V)echo -e "auto usb0\n" >> $(INTERFACES)
ifeq ($(CONFIG_FS_TARGET_NFSROOT),y)
	$(V)echo "#SDK configured for NFS root, so we don't mess with usb0 ipv4" >> $(INTERFACES)
	$(V)echo "iface usb0 inet manual" >> $(INTERFACES)
else
ifneq ($(CONFIG_BSP_NET_USB0_USE_DHCP),y)
	$(V)echo "iface usb0 inet static" >> $(INTERFACES)
	$(V)echo "	netmask $(CONFIG_BSP_NET_USB0_NETMASK)" >>$(INTERFACES)
else
	$(V)echo "iface usb0 inet dhcp" >> $(INTERFACES)
	$(V)echo "	script /etc/udhcp.script" >> $(INTERFACES)
endif #DHCP
endif #NFSROOT

ifeq ($(CONFIG_BSP_IPV6_SUPPORT),y)
ifeq ($(CONFIG_BSP_NET_USB0_USE_DHCPV6),y)
	$(V)echo "iface usb0 inet6 dhcp" >> $(INTERFACES)
endif # USB0_DHCPV6
ifeq ($(CONFIG_BSP_NET_USB0_USE_STATIC6),y)
	$(V)echo "iface usb0 inet6 static" >> $(INTERFACES)
	$(V)echo "	address $(CONFIG_BSP_NET_ETH0_GLOBAL_IPADDR6)/64" >>$(INTERFACES)
	$(V)echo "	gateway $(CONFIG_BSP_NET_ETH0_GATEWAY_IPADDR6)" >>$(INTERFACES)
endif # USB0_STATIC6
ifeq ($(CONFIG_BSP_NET_USB0_USE_STATELESS),y)
	$(V)echo "iface usb0 inet6 manual" >> $(INTERFACES)
endif # USB0_STATELESS
endif # IPV6_SUPPORT

endif #USB0

endif # HAS_NET

install: rrsdk_install

clean:
	@echo "rw-partition is clean" 

INIT_SCRIPT= rw-partition.init
INIT_SCRIPT_LEVEL=29

