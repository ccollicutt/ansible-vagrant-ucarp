# ucarp

This is a ucarp demo environment. Ucarp is interesting...it comes from OpenBSD's carp system I believe. Yay OpenBSD! But in this repository we're using Ubuntu Linux.

Using Vagrant and Ansible (as per usual) this repository will bring up three small Ubuntu Trusty nodes and configure them using Ucarp to have a couple of virtual IP addresses...one on the "external" network and one on the "internal" network.

## Run

Clone this playbook. Then just run vagrant up.

```bash
curtis$ vagrant up
```

After that you can either use "vagrant provision" or "ansible-playbook site.yml".

## Play

Once Vagrant and Ansible complete, you should be able to do this:

```bash
$ ansible -a "ifconfig eth2:ucarp" all
zone2 | success | rc=0 >>
eth2:ucarp Link encap:Ethernet  HWaddr 08:00:27:a6:62:a5  
UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1

zone0 | success | rc=0 >>
eth2:ucarp Link encap:Ethernet  HWaddr 08:00:27:e5:27:e0  
inet addr:172.200.0.10  Bcast:172.200.0.10  Mask:255.255.255.255
UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1

zone1 | success | rc=0 >>
eth2:ucarp Link encap:Ethernet  HWaddr 08:00:27:28:08:17  
UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1

curtis@parker:~/vagrant/ucarp$
```

and this:

```bash
$ ansible -a "ifconfig eth1:ucarp" all
zone0 | success | rc=0 >>
eth1:ucarp Link encap:Ethernet  HWaddr 08:00:27:76:09:41  
inet addr:172.100.0.10  Bcast:172.100.0.10  Mask:255.255.255.255
UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1

zone1 | success | rc=0 >>
eth1:ucarp Link encap:Ethernet  HWaddr 08:00:27:e1:ae:0d  
UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1

zone2 | success | rc=0 >>
eth1:ucarp Link encap:Ethernet  HWaddr 08:00:27:f1:31:30  
UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
```

If so, that is good.

Next you can start pinging 172.100.0.10. Then open up virtual box, go to the zone server that has the virtual IP assigned (in the above it's zone0) and click on settings. Then go to "network" -> "Adapter 2" and deselect cable connected and click "Ok" then the virtual IP should move to another host. Don't forget to re-check "Cable Connected". (ASIDE: I looked up doing this with vboxmanager but received an error message.) The pings should only stop for a second or two.

Now, in this example, zone2 has the vip.

```bash
$ ansible -a "ifconfig eth1:ucarp" all
zone2 | success | rc=0 >>
eth1:ucarp Link encap:Ethernet  HWaddr 08:00:27:f1:31:30  
inet addr:172.100.0.10  Bcast:172.100.0.10  Mask:255.255.255.255
UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1

zone1 | success | rc=0 >>
eth1:ucarp Link encap:Ethernet  HWaddr 08:00:27:e1:ae:0d  
UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1

zone0 | success | rc=0 >>
eth1:ucarp Link encap:Ethernet  HWaddr 08:00:27:76:09:41  
UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
```

That's it. Pretty straighfoward to configure virtual IPs.
