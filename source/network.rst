*************************
网络特性
*************************


.. csv-table::
   :header: Feature, Intel, Kunpeng
   :widths: 30, 20, 20

   AES-SMAC [#macsec]_ ,     支持,     支持
   VxLAN [#vxlan]_ ,        支持,    支持


AES-SMAC 验证

.. code-block:: console

    sudo ip link add link enp189s0f0 macsec0 type macsec port 11 encrypt on

    [user1@localhost ~]$ ip all
    12: macsec0@enp189s0f0: <BROADCAST,MULTICAST> mtu 1468 qdisc noop state DOWN group default qlen 1000
        link/ether 00:18:2d:04:00:5c brd ff:ff:ff:ff:ff:ff


    [user1@localhost ~]$ ip macsec show
    12: macsec0: protect on validate strict sc off sa off encrypt on send_sci on end_station off scb off replay off
        cipher suite: GCM-AES-128, using ICV length 16
        TXSC: 00182d04005c000b on SA 0

VxLAN 验证

.. code-block:: console

    ip link add vxlan0 type vxlan id 42 group 239.1.1.1 dev enp189s0f0 dstport 4789

    [user1@localhost ~]$ ip all
    13: vxlan0: <BROADCAST,MULTICAST> mtu 1450 qdisc noop state DOWN group default qlen 1000
    link/ether 26:e0:63:ef:9a:36 brd ff:ff:ff:ff:ff:ff


.. [#macsec] http://man7.org/linux/man-pages/man8/ip-macsec.8.html
.. [#vxlan] https://www.kernel.org/doc/Documentation/networking/vxlan.txt
