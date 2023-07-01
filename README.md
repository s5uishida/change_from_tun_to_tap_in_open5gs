# A Note for Changing Network Interface of UPF from TUN to TAP in Open5GS

Open5GS UPF operates a network interface of which name containing the "tap" string as a TAP interface.
The default is a TUN interface.

An example when the network interface name is `ogstap` is as follows.

<h2 id="change_network">Changes of the network settings in UPF machine</h2>

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

<h2 id="change_upf">Changes of the configuration for UPF</h2>

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
