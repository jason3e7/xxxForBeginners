### 第一次實作
# [SSRF](https://www.wikiwand.com/en/Server-side_request_forgery)
### 就上手

jason3e7 20181204

Note:title:"第一次實作SSRF就上手"

---

# define:SSRF
* Server-Side Request Forgery
* 服務端提供了從其他服務器應用獲取數據的功能, 如果沒有防禦機制, 可建構攻擊語法.
  * 讓服務器去請求你通常請求不到的東西

---

# SSRF 概念圖解
![](ssrf/image/image2.png)

Note:https://www.acunetix.com/blog/articles/server-side-request-forgery-vulnerability/

---

# SSRF 可以達成
* [跨協議通信技術利用](https://www.freebuf.com/articles/web/19622.html)
* 從主機的位置去打內網.(GET)
* 利用file協議讀取本地文件.(或者其他協議)

Note:
URL結構
`scheme://user:pass@host:port/path?query=value#fragment`

# ref
* [[安全科普]SSRF攻擊實例解析](https://www.freebuf.com/articles/web/20407.html)
* [SSRF bible. Cheatsheet](https://repo.zenk-security.com/Techniques%20d.attaques%20%20.%20%20Failles/SSRFbible%20Cheatsheet.pdf)
* [bugbounty-cheatsheet](https://github.com/EdOverflow/bugbounty-cheatsheet/blob/master/cheatsheets/ssrf.md)
* [通過拆分攻擊實現的SSRF攻擊](https://xz.aliyun.com/t/2894), 什麼是拆分攻擊!?
* [SSRF攻擊原理與技巧](https://www.jianshu.com/p/612c010e588e), 什麼是 DNS Rebinding !?
* [SSRF攻擊文檔翻譯](https://evilwing.me/2018/07/01/ssrf%E6%94%BB%E5%87%BB-%E8%AF%91%E6%96%87/)
* [淺析SSRF原理及利用方式](https://www.anquanke.com/post/id/145519), 有一些 redis 案例.

---

## SSRF 跨協議通信技術利用
* 內網、本地端口掃瞄，獲取一些服務的banner信息
  * http://127.0.0.1:3306/test.txt, 會掃描 3306 port.
* 攻擊運行在內網或本地的應用程序
  * [攻擊應用程序](https://www.freebuf.com/articles/web/20407.html)

---

## SSRF 從主機的位置去打內網
* 對內網web應用進行指紋識別, 通過訪問默認文件實現.
  * [內網web應用指紋識別](https://www.freebuf.com/articles/web/20407.html)
* 攻擊內外網的web應用, 主要是使用get參數就可以實現的攻擊.

---

# SSRF scheme
* 利用file協議讀取本地文件.(或者其他協議)
  * file:///etc/passwd

---

# 危險的 php function
* file_get_contents
* fsockopen
* curl_exec

Note:[SSRF 服務端請求偽造](https://ctf-wiki.github.io/ctf-wiki/web/ssrf/)

---

# file_get_contents
```
<?php
if (isset($_POST['url'])) { 
    $content = file_get_contents($_POST['url']); 
    $filename ='./images/'.rand().';img1.jpg'; 
    file_put_contents($filename, $content); 
    echo $_POST['url']; 
    $img = "<img src=\"".$filename."\"/>"; 
}
echo $img;
?>
```

---

# fsockopen
```
<?php 
function GetFile($host,$port,$link) { 
    $fp = fsockopen($host, intval($port), $errno, $errstr, 30); 
    if (!$fp) { 
        echo "$errstr (error number $errno) \n"; 
    } else { 
        $out = "GET $link HTTP/1.1\r\n"; 
        $out .= "Host: $host\r\n"; 
        $out .= "Connection: Close\r\n\r\n"; 
        $out .= "\r\n"; 
        fwrite($fp, $out); 
        $contents=''; 
        while (!feof($fp)) { 
            $contents.= fgets($fp, 1024); 
        } 
        fclose($fp); 
        return $contents; 
    } 
}
?>
```

Note:相對不熟的功能.

---

# curl_exec
```
<?php 
if (isset($_POST['url'])) {
    $link = $_POST['url'];
    $curlobj = curl_init();
    curl_setopt($curlobj, CURLOPT_POST, 0);
    curl_setopt($curlobj,CURLOPT_URL,$link);
    curl_setopt($curlobj, CURLOPT_RETURNTRANSFER, 1);
    $result=curl_exec($curlobj);
    curl_close($curlobj);

    $filename = './curled/'.rand().'.txt';
    file_put_contents($filename, $result); 
    echo $result;
}
?>
```

---

# The End
