# A Note for Changing Network Interface of UPF from TUN to TAP in Open5GS

---

### [Sample Configurations and Miscellaneous for Mobile Network](https://github.com/s5uishida/sample_config_misc_for_mobile_network)

---

Open5GS UPF operates a network interface of which name containing the "tap" string as a TAP interface.
The default is a TUN interface.

An example when the network interface name is `ogstap` is as follows.

<a id="change_network"></a>

## Changes of the network settings in UPF machine

```diff
-ip tuntap add name ogstun mode tun
-ip addr add 10.45.0.1/16 dev ogstun
-ip link set ogstun up
+ip tuntap add name ogstap mode tap
+ip addr add 10.45.0.1/16 dev ogstap
+ip link set ogstap up
 
-iptables -t nat -A POSTROUTING -s 10.45.0.0/16 ! -o ogstun -j MASQUERADE
+iptables -t nat -A POSTROUTING -s 10.45.0.0/16 ! -o ogstap -j MASQUERADE
```

<a id="change_upf"></a>

## Changes of the configuration for UPF

- `open5gs/install/etc/open5gs/upf.yaml`
```diff
--- upf.yaml.orig       2023-07-02 01:46:43.934079072 +0900
+++ upf.yaml    2023-07-02 01:46:49.334083190 +0900
@@ -201,6 +201,7 @@
       - addr: 127.0.0.7
     subnet:
       - addr: 10.45.0.1/16
+        dev: ogstap
     metrics:
       - addr: 127.0.0.7
         port: 9090
```
