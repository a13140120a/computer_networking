# computer_networking

* ## [introduction](#001) #



****

<h1 id="001">introduction</h1> 

* ## [Network edge](#0011) #



<h2 id="0011">Network edge</h2> 


* host、laptop等網路邊邊設備
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

* 網路核心部分
* 兩個點之間一定有至少兩條或以上的路線


* data 傳輸方式：
  * circuit switching：
    * 有一個專用電路，會保留專屬頻寬(End-end resources reserved for “call”)
    * no sharing
    * dividing link bandwidth into “pieces”
    * resource piece idle if not used by owning call
    * 又分成frequency division(FDMA)、time division(TDMA)
      * FDMA：以頻率切割
      * ![FDMA](https://github.com/a13140120a/computer_networking/blob/master/imgs/FDMA.PNG)
      * TDMA：以時間切割
      * ![TDMA](https://github.com/a13140120a/computer_networking/blob/master/imgs/TDMA.PNG)
    * 1Mb頻寬，每人100kbps使用量，active 時間只有10%，使用 TDMA 只能最多10人始用
  * packet-switching：
    * 把資料切成許多單位(packets)，一個一個送
    * 沒有固定線路，也沒有固定時間
    * 每個 packets 都是用全部頻寬在送
    * share network resource，resource 會有競爭問題
    * 來不及送的封包會 queue 在 buffer 裡面
    * 1Mb頻寬，每人100kbps使用量，active 時間只有10%，使用 packet-switching 35個人使用同時10個人使用的機率小於0.0004
    * 現今網路大部分使用 packet-switching
    * forwarding 的方式有兩種：
      * datagram network：
        * destination address  in packet determines next hop
        * routes may change during session
      * virtual circuit network：
        * 雖然不用專用的線路，但盡量走固定的線路
        * 每個封包有一個 tag，router 看到 tag 就知道要送往哪裡

<h2 id="0013">Access networks and physical media</h2> 


* Residential access：
  * point to point access：
    * Dialup via modem(撥接網路)：
      * 老舊方法，速度慢，不能同時上網與講電話
    * ADSL: asymmetric digital subscriber line
      * asymmetric 的意思就是上傳與下載速度不一樣。
      * [FDM](https://zh.wikipedia.org/zh-tw/%E9%A2%91%E5%88%86%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8)
  * cable modems：
    * HFC: hybrid fiber coaxial cable(光纖與銅軸混合)
* Company access：
  * local area networks(LAN，區域網路)：
    * Ethernet：最常用
* Wireless access networks：
  * wireless LANs:
    * 802.11b (WiFi): 11 Mbps
    * 802.11g: 54 Mbps
    * 802.11n: 108 Mbps
  * wider-area wireless access：
    * 3G、4G
    * 802.16a/e (WiMax)

<h2 id="0014">Encapsulation</h2> 

* application 層的單位叫做 Message
* transport 層的單位叫做 segment
* network 層的單位叫做 datagram
* link 層的單位叫做 frame


<h1 id="002">Application layer</h1> 



<h2 id="0021">overview</h2> 

* Some network apps：
  * E-mail
  * Web
  * Instant messaging
  * Remote login
  * P2P file sharing
  * Multi-user network games
  * Streaming stored video clips
  * Internet telephone
  * Real-time video conference
  * Massive parallel computing

* Application architectures：
  * Client-server
  * Peer-to-peer (P2P)：
    * example: Gnutella
    * difficult to manage
    * Highly scalable
  * Hybrid of client-server and P2P(混合式)
    * Napster：
      * 要檔案的時候是跟很多個 peer 同時要
      * 上線時或搜尋檔案是centralized(需要server)
    * Instant messaging：
      * Chatting between two users is P2P
      * Presence detection/location centralized

* Processes communicating：
  * 如果 processes 在相同的電腦溝通使用 inter-process communication(IPC)
  * 如果 processes 在相同的電腦溝通使用 [message](#0014)
  * peer 既是 server 也是 client
  * processes 使用 socket 來接收/傳送 message
    * ![Application_processes_sockets_and_underlying_transport_protocol](https://github.com/a13140120a/computer_networking/blob/master/imgs/Application_processes_sockets_and_underlying_transport_protocol.PNG)

* Addressing processes：
  * 靠 IP 找 host
  * 靠 port 找 process




<h2 id="0022">Web and HTTP</h2> 

* Web page：
  * consists of objects
  * an object is a file such as an HTML file, a JPEG image, a Java applet, an audio file,…
  * A Web page consists of a base HTML-file and several referenced objects
  * The base HTML file references the other objects in the page with the object’s URLs (Uniform Resource Locators)
* HTTP: hypertext transfer protocol
  * HTTP is “stateless”：server maintains no information about past client requests
  * 過程：
    * client 發起 TCP 連線(creates socket)
    * server accepts TCP connection from client
    * HTTP messages (application-layer protocol messages) exchanged between browser (HTTP client) and Web server (HTTP server)
    * TCP connection closed
  * HTTP/1.0 
    * uses nonpersistent HTTP
    * nonpersistent 就是建完連線就關掉
    * 一個 TCP 連線最多只送一個 object
    * Method Type：
      * GET : Return the object 
      * POST : Send information to be stored on the server 
      * HEAD：Return only information about the object, such as how old it is, but not the object itself
  * HTTP/1.1 
    * persistent 就是建完連線就放在那裏
    * uses persistent connections in default mode – 
    * 可以手動設定成使用 nonpersistent connection
    * 一個 TCP 連線可以送很多個 objects
    * Method Type：
      * GET, POST, HEAD
      * PUT：Uploads a new copy of existing object in entity body to path specified in URL field
      * DELETE：deletes object specified in the URL field

  * [HTTP/2](https://notfalse.net/39/http-message-format)

* RTT：Round-trip delay time
  * time to send a small packet to travel from client to server and back.

* HTTP request：
  * HTTP/1.0 Method Type：
    * GET : Return the object 
    * POST : Send information to be stored on the server 
    * HEAD：Return only information about the object, such as how old it is, but not the object itself
  * HTTP/1.1 Method Type：
    * GET, POST, HEAD
    * PUT：Uploads a new copy of existing object in entity body to path specified in URL field
    * DELETE：deletes object specified in the URL field
  * message_format：
    * ![HTTP_request_message_format](https://github.com/a13140120a/computer_networking/blob/master/imgs/HTTP_request_message_format.PNG)

* HTTP response：
  * [status code](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Status)
  * message_format：
    * ![HTTP_response_message](https://github.com/a13140120a/computer_networking/blob/master/imgs/HTTP_response_message.PNG)

* `telnet www.eurecom.fr 80`：與 www.eurecom.fr port 80 建立 telnet 連線
* `GET www.google.com HTTP/1.1`：使用 GET 方法去 request
* cookies：
  * Four components of cookie technology:
    * cookie header line in the HTTP response message
    * cookie header line in HTTP request message
    * cookie file kept on user’s host and managed by user’s browser
    * back-end database at Web site

<h2 id="0023">FTP</h2> 

* FTP: the file transfer protocol

* 分成 TCP control connection 還有 TCP data connection：
  * TCP control connection： port 21
    * [FTP Client Commands](https://www.ibm.com/docs/en/scbn?topic=SSRJDU/gateway_services/ftp_globalec/SCN_Summary_of_FTP_Client_Commands_b.html)
    * [FTP server return codes](https://en.wikipedia.org/wiki/List_of_FTP_server_return_codes)
  * TCP data connection： port 20




<h2 id="0024">Electronic Mail</h2> 

* SMTP, POP3, IMAP



<h2 id="0025">Electronic Mail</h2> 











