# computer_networking

* ## [introduction](#001) #
* ## [Application layer](#002) #


****

<h1 id="001">introduction</h1> 

* ## [Network edge](#0011) #
* ## [Network core](#0012) #
* ## [Access networks and physical media](#0013) #
* ## [Encapsulation](#0014) #
* ## [RTT](#0015) #


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
      * [FDM(把線路依照頻率切割成很多頻道)](https://zh.wikipedia.org/zh-tw/%E9%A2%91%E5%88%86%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8)
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

<h2 id="0015">RTT</h2> 

* RTT：Round-trip delay time
  * time to send a small packet to travel from client to server and back.


<h1 id="002">Application layer</h1> 


* ## [overview](#0021) #
* ## [Web and HTTP](#0022) #
* ## [FTP](#0023) #
* ## [Encapsulation](#0014) #

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
  * 由 objects 組成
  * object 可能是 file (例如 HTML file, JPEG image, Java applet, audio file,…)
  * Web page 是由 base HTML-file 和若干個 referenced objects 所組成
  * base HTML file 藉由 object 的 URLs(Uniform Resource Locators) references 到其他的 objects
* HTTP: hypertext transfer protocol 
  * Base on TCP
  * HTTP is “stateless”：server 不會 maintains 所有之前的 client requests 的 information
  * 過程：
    * client 發起 TCP 連線(creates socket)
    * server accepts TCP connection from client
    * HTTP messages (application-layer protocol messages) 在 browser (HTTP client) 與 Web server (HTTP server) 之間交換訊息
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

* HTTP request：
  * HTTP/1.0 Method Type：
    * GET : Return the object 
    * POST : Send information to be stored on the server 
    * HEAD：Return only information about the object(Metadata), such as how old it is, but not the object itself
  * HTTP/1.1 Method Type：
    * GET, POST, HEAD
    * PUT：Uploads a new copy of existing object in entity body to path specified in URL field(update 一個新版本的 object 再貼回去)
    * DELETE：deletes object specified in the URL field
  * message_format：
    * ![HTTP_request_message_format](https://github.com/a13140120a/computer_networking/blob/master/imgs/HTTP_request_message_format.PNG)

* HTTP response：
  * [status code](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Status)
  * message_format：
    * ![HTTP_response_message](https://github.com/a13140120a/computer_networking/blob/master/imgs/HTTP_response_message.PNG)

* `telnet www.eurecom.fr 80`：與 www.eurecom.fr port 80 建立 telnet 連線
* `GET www.google.com HTTP/1.1`：使用 GET 方法去 request


* Authorization : control access to server content
  * stateless: 因為是 stateless 所以每個 request 都必須包含 Authorization 在裡面
  * 當 browser 保持打開狀態時，用戶名和密碼會被 cache，因此不會提示用戶為每個請求輸入用戶名和密碼


* cookies：
  * Four components of cookie technology:
    * cookie header line in the HTTP response message (訪問網站的時候一開始 web server 會先給你一個 cookie)
    * cookie header line in HTTP request message (往後訪問網站的時候就可以帶著這個拿到的 cookie 去 request)
    * cookie file kept on user’s host and managed by user’s browser (cookie 通常會由 user 的 browser 保管)
    * back-end database at Web site (網站的 database 也會保存一份 cookie 這樣 user 來訪問時，就可以去查詢並比對)
  * what cookie can bring：
    * authorization
    * shopping carts
    * recommendations
    * user session state (Web e-mail)

* Web caches (proxy server)：
  * 可以做流量控管
  * 可以增加 request 的速度 (proxy server 如果架在 client 端的區網的話，就可以避免 server 太多人訪問而造成速度下降的問題)
  * ![proxy_server](https://github.com/a13140120a/computer_networking/blob/master/imgs/proxy_server.PNG)



<h2 id="0023">FTP</h2> 

* FTP: the file transfer protocol
* FTP server 會 maintain "state"

* FTP 要建兩個連線，分成 TCP control connection 還有 TCP data connection：
  * TCP control connection： port 21
    * [FTP Client Commands](https://www.ibm.com/docs/en/scbn?topic=SSRJDU/gateway_services/ftp_globalec/SCN_Summary_of_FTP_Client_Commands_b.html)
    * [FTP server return codes](https://en.wikipedia.org/wiki/List_of_FTP_server_return_codes)
  * TCP data connection： port 20




<h2 id="0024">Electronic Mail</h2> 

* SMPT：port 25，用於 mail server 與 mail server 之間的溝通。
  * 分成三步驟：
    * handshaking (greeting)
    * transfer of messages
    * closure
  * commands: 7-bit ASCII
  * response: status code and phrase
* [MIME](https://zh.wikipedia.org/zh-tw/%E5%A4%9A%E7%94%A8%E9%80%94%E4%BA%92%E8%81%AF%E7%B6%B2%E9%83%B5%E4%BB%B6%E6%93%B4%E5%B1%95#MIME_headers)：多用途網際網路郵件擴展（Multipurpose Internet Mail Extensions）是一個網際網路標準，它擴展了電子郵件標準

* Mail access protocols：
  * user 存取 mail server 的方式(protocal)
  * 有以下三種：
    * HTTP, POP3, IMAP

<h2 id="0024">DNS</h2> 

* DNS(Domain Name System)：
  * 將 domain name map 到 IP address
  * 實作於 applocation layer 
  * 提供 core Internet function 
  * 是一個「複雜的設計應該在"edge"，越接近 core 應該越簡單」的設計的例子。
  * 是一個 distributed database implemented in hierarchy of many name servers
    * ![DNS](https://github.com/a13140120a/computer_networking/blob/master/imgs/DNS.PNG)
    * 分成四種類型的 name service：
      * root name servers：Client queries a root server to find "com" DNS server
      * top level name servers(頂級域, Top-level Domain, TLD）)：Client queries com DNS server to get (例如)amazon.com DNS server
      * authoritative name servers：Client queries amazon.com DNS server to get  IP address for www.amazon.com (真正有儲存 name 和 IP 的 server)
      * local name servers：each ISP, company has local (default) name server(host 的 DNS query 會先到 local name servers 去查，查不到的話才會往 root server 去查)
    * [全球13個 root name servers](https://zh.wikipedia.org/zh-tw/%E6%A0%B9%E7%B6%B2%E5%9F%9F%E5%90%8D%E7%A8%B1%E4%BC%BA%E6%9C%8D%E5%99%A8#%E7%AE%A1%E7%90%86%E6%9C%BA%E6%9E%84%E5%8F%8A%E8%AE%BE%E7%BD%AE%E5%9C%B0%E7%82%B9)

* aliasing：
  * Host aliasing：
    * 又分成 Canonical 和 alias names：
      * Canonical：較正規的、正式的，例如：Relay1.west-coast.enterprise.com
      * alias：較友善、好記的，例如：enterprise.com 或者 www.enterprise.com
    * 一個 Canonical name 可以對應到好多個不同的 IP ，如此一來便可以分散流量
  * Mail server aliasing：
    * 例如：**Relay1.west-coast.hotmail.com** 就會變成 **bob@hotmail.com**

* Recursive and iterated queries：
  * Recursive：當一個 domain name server 查不到該網域時，他會幫你去查
  * iterated：當一個 domain name server 查不到該網域時，他會直接跟你說他查不到
  * ![dns_recursive_iter](https://github.com/a13140120a/computer_networking/blob/master/imgs/dns_recursive_iter.PNG)

* Caching：
  * DNS 會把以前查過的 IP 跟 Domain name 存起來
  * 每個 cache 都會有一個 timeout 的時間(通常是兩天)，超過這個時間就會被刪除
  * RR(resource record)：DNS 中的每一筆紀錄都叫做 RR，其 format 就像 (name, value, type, ttl(timeout 時間))，RR 又可以依照 type 分成以下四種：
    * Type=A：
      * name = host name，value = IP address，例如 (relay1.bar.foo.com, 145.37.93.126, A, ttl)
    * Type=NS：
      * name = domain(e.g. foo.com)，value = host name(domain 所對應到的 host name，例如 dns.foo.com)，一筆紀錄就像 (foo.com, dns.foo.com, NS)
    * Type=CNAME：
      * value = canonical name，name 是 value 所對應的 alias name，例如 (foo.com, relay1.bar.foo.com, CNAME)
    * Type=MX：
      * mail 專用的 type，name = alias name for some mail server，value = the canonical name of the mail server，例如 (foo.com, mail.bar.foo.com, MX)

* 封包格式：
  * query 跟 reply 使用的是相同的格式。
  * ![DNS_message_format](https://github.com/a13140120a/computer_networking/blob/master/imgs/DNS_message_format.png)
  * identification：因為一次可能同時發出很多的 query，這個欄位就是為了能夠分辨 reply 是屬於哪個 query 的(發出去的 query 與對應的 reply message id 相同)。
  * flag：這個欄位用來標註此封包是 query 還是 reply ，如果是 query 是不是需要 recursion，又如果是 reply 的話，recursion 可不可用。
  * questions：帶著 RR 的 name 跟 type 去查詢
  * answer：response to query(一個 hostname 可以對應多個 IP)
  * 


<h2 id="0024">P2P File sharing</h2> 

* 當 user(也就是其中一個 peer)想要獲得一份檔案的時候，可以藉由其他 peer 來獲得檔案的各個部份，以此減輕 server 的負擔，或甚至不需要 server
* 分成 centralized directory 跟 Query flooding：
  * centralized directory：
    * 依靠一個 central server 來提供每個 query 哪個檔案的哪個部分放在哪個 IP 的 Host 上面，
    * 缺點是當 central server 壞掉時，整個 P2P 都會 Crash 掉。
    * 另一個缺點是會有效能的瓶頸
  * Query flooding(以 Gnutella 為例)：
    * 不需要 central server
    * 當某個 user query 某個檔案的部分的時候，user host 會向自己所連接的其他 peer 發出 query，如果這個 peer 沒有的話，再詢問另一個 peer(這種行為就稱為 Query flooding)。
    * 如果找到需要的檔案部分的話，稱為 query hit。
    * flooding 會有一些避免 query 氾濫的機制，例如最多連接到第幾次，或者檢查該 query 是否已經處理過了。

* peer join(以 Gnutella 為例)：
  * 首先欲加入的 peer x 會先有一個 candidate peers 的清單
  * x 會一個一個地嘗試與清單中的 peer 連線，先 ping 看看，如果有回應的話就建 TCP 連線。

* KaZaA：
  * 每個 peer 型成一個一個的 group，然後一個 group 會有一個 leader
  * leader 之間彼此連線
  * 每個 group 的成員都連接到 leader
  * leader 會有每個 group children 存放那些檔案部分的資訊
  * ![KaZaA](https://github.com/a13140120a/computer_networking/blob/master/imgs/KaZaA.PNG)
  * 每個 file 都會有一個 hash 值和一個 descriptor(類似 metadata)，client 送出 keyword 讓 leader 去查詢(利用 hash 和 descriptor 來做比對)，然後傳回 match 的資訊，之後 client 會從這些資訊當中去找擁有檔案的 peer 來下載。





<h2 id="0025">Electronic Mail</h2> 











