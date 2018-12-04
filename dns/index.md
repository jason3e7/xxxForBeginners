### 第一次學習
# DNS
### 就上手

jason3e7 20181204

Note:title:"第一次學習DNS就上手"

---

# define:DNS
* Domain Name System
* 網際網路的一項服務
  * 它作為將域名和IP位址相互對映的一個分散式資料庫

---

# [DNS運作步驟](http://dns-learning.twnic.net.tw/dns/03opDNS.html#two) #
```
* 客戶端向伺服器提出查詢項目﹔
* 當被詢問到有關本域名之內的主機名稱的時候，DNS伺服器會直接做出回答﹔
* 如果所查詢的主機名稱屬於其它域名的話，會檢查快取記憶體(Cache)，看看有沒有相關資料﹔
* 如果沒有發現，則會轉向root伺服器查詢﹔
* 然後root伺服器會將該域名之下一層授權(authoritative)伺服器的位址告知(可能會超過一台)﹔
* 本地伺服器然後會向其中的一台伺服器查詢，並將這些伺服器名單存到記憶體中，以備將來之需(省卻再向root查詢的步驟)﹔
* 遠方伺服器回應查詢﹔
* 若該回應並非最後一層的答案，則繼續往下一層查詢，直到獲得客戶端所查詢的結果為止﹔
* 將查詢結果回應給客戶端，並同時將結果儲存一個備份在自己的快取記憶裡面﹔
* 如果在存放時間尚未過時之前再接到相同的查詢，則以存放於快取記憶裡面的資料來做回應。
```

---

# DNS attack
* DNS hijacking 
  * 控制 DNS 的 Response 結果
  * DNS record hijacking !?
* DNS redirection
  * 控制 DNS server ip
  * DNS server redirection !?
* DNS Spoofing(DNS Cache Poisoning) 
  * 做假的 DNS server(DNS record)去污染其他的 DNS server.
  * DNS record Spoofing !?

Note:所以 DNS redirection 可以引發 DNS hijacking !?

---

## DNSSEC ##
* DNS 資安解決方案

---

# The End
