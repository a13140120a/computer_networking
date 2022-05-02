# computer_networking

* ## [introduction](#001) #



****

<h1 id="001">introduction</h1> 

* ## [Network edge](#0011) #



<h2 id="0011">Network edge</h2> 

* client/server model
  * email, web
* peer-to-peer model
  * Gnutella, KaZaA

* connection-oriented service
  * TCP: 
    * Handshaking
    * reliable,
    * in-order byte-stream data transfer
    * flow control(不會塞爆 receiver)
    * congestion control(不會造成 network 擁塞)
    * 使用情境：HTTP (Web), FTP (file transfer), Telnet (remote login), SMTP (email)
    * 
* connectionless service
  * UDP
    * unreliable
    * no flow control
    * no congestion control
    * 使用情境：streaming media, teleconferencing, DNS, Internet telephony



<h2 id="0012">Network core</h2> 

* 兩個點之間一定有至少兩條或以上的路線


* data 傳輸方式：
  * circuit switching：
    * 有一個專用電路，會保留專屬頻寬(End-end resources reserved for “call”)
    * no sharing
    * dividing link bandwidth into “pieces”
    * resource piece idle if not used by owning call
    * 又分成frequency division(FDMA)、time division(TDMA)
      * 



  * packet-switching：
    * 把資料切成許多單位，一個一個送
    * 








