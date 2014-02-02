# Dynamic Routing

Dynamic Routing มีอัลกอริธึมอยู่สองรูปแบบคือ Distance Vector เช่น RIP และ Link State เช่น OSPF ซึ่งแบ่งตามลักษณะการทำงาน โดยใน Packet Tracer การทำ Dynamic Routing จะต้องกำหนด neighbor Router เพื่อให้เราท์เตอร์ไปทำงานต่อไป

```
R0#show ip route 
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/30 is subnetted, 2 subnets
C       10.10.10.0 is directly connected, Serial0/0/0 <- บอกว่าพอร์ตไหนต่อตรงกับพอร์ตไหน
C       10.10.10.8 is directly connected, Serial0/0/1 <- บอกว่าพอร์ตไหนต่อตรงกับพอร์ตไหน
C    192.168.0.0/24 is directly connected, FastEthernet0/0
R0#
```

## RIP

```
R0#conf t
R0(config)#router rip
R0(config-router)#?
  auto-summary         Enter Address Family command mode
  default-information  Control distribution of default information
  distance             Define an administrative distance
  exit                 Exit from routing protocol configuration mode
  network              Enable routing on an IP network
  no                   Negate a command or set its defaults
  passive-interface    Suppress routing updates on an interface
  redistribute         Redistribute information from another routing protocol
  timers               Adjust routing timers
  version              Set routing protocol version
R0(config-router)#
```

คำสั่งหลักๆ ที่ใช้คือ ```network``` และ ```version``` โดย ```network``` ให้พิมพ์ Network ID ของเน็ตเวิร์คที่ต่ออยู่ลงไป

```
R0(config-router)#network ?
  A.B.C.D  Network number
R0(config-router)#net
R0(config-router)#network 192.168.0.0 <--- เป็นการบอกว่าจะ run RIP ที่ interface ใดบ้าง
R0(config-router)#network 10.10.10.0
R0(config-router)#network 10.10.10.8
R0(config-router)#
```

โดยต้องทำกับเราท์เตอร์อื่นด้วย (R1)

```	
R1#sh ip ro
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/30 is subnetted, 1 subnets
C       10.10.10.0 is directly connected, Serial0/0/0
C    192.168.1.0/24 is directly connected, FastEthernet0/0
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#router rip
R1(config-router)#network 192.168.1.0
R1(config-router)#network 10.10.10.0
R1(config-router)#exit
R1(config)#
```

เมื่อดู ```ip route``` จะเห็นว่ามีการเพิ่มเข้ามาจากเดิม

```
R1(config)#do sh ip ro
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/30 is subnetted, 2 subnets
C       10.10.10.0 is directly connected, Serial0/0/0
R       10.10.10.8 [120/1] via 10.10.10.1, 00:00:07, Serial0/0/0
R    192.168.0.0/24 [120/1] via 10.10.10.1, 00:00:07, Serial0/0/0
C    192.168.1.0/24 is directly connected, FastEthernet0/0
R1(config)#
```

สามารถอธิบายได้ว่า หากมีแพ็คเกตจะออกไปยัง **192.168.0.0**, **10.10.10.8** จะออกที่ขา **10.10.10.1** โดยมี Next hop คือ **Serial0/0/0**

โดยรูปแบบของ RIP ที่ทำไปนี้เป็น RIPv1 ซึ่งจะเป็น classful ไม่มีการส่ง Subnet Mask โดยจะใช้ Default Subnet Mask อย่างเดียว เมื่อมีการเปลี่ยนแปลง เช่น การเพิ่ม Subnet แล้วอาจเกิดปัญหาในการส่งข้อมูลได้ ซึ่งสามารถแก้ปัญหาได้จากเปลี่ยนไปใช้ RIPv2 โดยใช้คำสั่ง ```version 2```

```
R4(config)#no router rip
R4(config)#router rip
R4(config-router)#ver
R4(config-router)#version 2
R4(config-router)#network 10.10.10.20
R4(config-router)#
```

คำสั่ง ```no auto-summary``` คือการไม่ให้ประกาศ Summary Route โดยอัตโนมัติ

คำสั่ง ```passive-interface``` ตามด้วย interface (e.g. passive-interface fa0/0) คือการไม่ให้ router ส่ง routing information ไปยัง interface นั้นๆ (เช่น interface นั้นไม่ได้ต่อกับ router)

## OSPF

ทำได้โดยใช้คำสั่ง ```router ospf 1```

```
R0(config)#router ospf 1
R0(config-router)#?
  area                   OSPF area parameters
  default-information    Control distribution of default information
  distance               Define an administrative distance
  exit                   Exit from routing protocol configuration mode
  log-adjacency-changes  Log changes in adjacency state
  network                Enable routing on an IP network
  no                     Negate a command or set its defaults
  passive-interface      Suppress routing updates on an interface
  redistribute           Redistribute information from another routing protocol
  router-id              router-id for this OSPF process
R0(config-router)#
```

โดยใน OSPF นั้นต้องการ OSPF wild card bits (Invert Subnet Mask) เช่น

- /24 : 255.255.255.0 then Wild Card Bits: 0.0.0.255

- /25 : 255.255.255.128 then Wild Card Bits: 0.0.0.127

- /30 : 255.255.255.252 then Wild Card Bits: 0.0.0.3

(เวลา Invert เปรียบได้กับการเอา 255 มาลบกับ Subnet Mask)

และยังต้องใส่ Area ID ที่จะให้ OSPF ทำงานด้วย

```
R0(config-router)#network ?
  A.B.C.D  Network number
R0(config-router)#network 192.168.0.0 ?
  A.B.C.D  OSPF wild card bits
R0(config-router)#network 192.168.0.0 0.0.0.127 ?
  area  Set the OSPF area ID
R0(config-router)#network 192.168.0.0 0.0.0.127 area 0
R0(config-router)#network 10.10.10.0 0.0.0.3 area 0
R0(config-router)#
.
.
.
R1(config)#router ospf 1
R1(config-router)#
R1(config-router)#network 192.168.1.0 0.0.0.255 area 0
R1(config-router)#network 10.10.10.0 0.0.0.3 area 0
```

ในตัวอย่างจะกำหนดเป็น area 0 และทำให้ครบทุกขาเหมือน RIP โดยคำสั่งที่ใช้ในการลบคอนฟิกแบบ OSPF จะเหมือนกับ RIP

```
R0(config)#no router ospf 1
```
