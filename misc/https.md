# HTTPS

证书后面有证书内容的Hash

* 机构对Hash使用私钥进行签名
* 客户端用公钥对签名进行验证

**第一步：**浏览器与某宝建立TCP连接**第二步：**服务器会弹出一个页面提醒安装数字证书，如果不安装，接下来一切都不会顺利进行**第三步：**浏览器需要认证某宝是真实的服务器（不是山寨的），服务器发来了自己的**数字证书**。**插一句：某宝的数字证书从哪里来？**某宝自己的认证中心简称**CA（Certificate Authority）**，CA给某宝颁发了一个证书，这个证书有：签发者证书用途某宝的公钥某宝的加密算法某宝用的HASH算法证书的到期时间等**如果证书就这样给某宝了，那传输过程中如果有人篡改这个证书，那这个证书还有什么权威性？**简单的很，把以上内容做一次HASH，得到一个固定长度（比如128位的HASH，然后再用CA的私钥加密，就得到了**数字签名**，附在以上证书的末尾，一起传输给某宝。设想一下，如果不加密那个HASH，任何人都可以先篡改证书，然后再计算HASH，附在证书的后面，传给某宝时，某宝无法发现是否有人篡改过。而用CA私钥加密后，就生成了类似人体指纹的签名，任何篡改证书的尝试，都会被数字签名发现。**第四步：**浏览器接到某宝的数字证书，从第二步得到的CA公钥值，可以解密数字证书末尾的**数字签名**（CA私钥加密，可以用CA公钥解密，此为非对称加密），得到原始的HASHs然后自己也按照证书的HASH算法，自己也计算一个HASHc，如果**HASHc== HASHs，则认证通过**，否则认证失败。假设认证成功，否则故事无法编下去了…**第五步：**双方会运行 Diffie Hellman 算法，简称 **DH算法**。通俗地说：双方会协商一个master-key，这个master-key 不会在网络上传输、交换，它们独立计算出来的，其值是相同的，只有它们自己双方知道，任何第三方不会知道，俗称的天不知，地不知，你知，我知。然后以master-key推导出 session-key，用于双方SSL数据流的加密/解密，采用对称加密，保证数据不被偷窥，加密算法一般用**AES。**以master-key推导出 hash-key，用于数据完整性检查（Integrity Check Verification）的加密密钥，HASH算法一般有：**MD5、SHA**，通俗滴说，保证数据不被篡改。**第六步：**然后就可以正常发送订单了，用HASH key 生成一个MAC（ Message Authentication Code），附在HTTP报文的后面，然后用session key 加密所有数据（HTTP + MAC），然后发送出去**第七步：**服务器先用session key 解密数据，得到HTTP + MAC，然后自己用相同的算法计算自己的MAC，如果两个MAC相等，则数据没有被篡改。**第八步：**所有购物安全无误地完成，手下给马老板买了一件神秘礼物，一件COS Play装&lt;img data-rawwidth="900" data-rawheight="435" src="https://pic1.zhimg.com/50/v2-9567684cec68d1618b7afac056c812a0\_hd.jpg" class="origin\_image zh-lightbox-thumb" width="900" data-original="https://pic1.zhimg.com/v2-9567684cec68d1618b7afac056c812a0\_r.jpg"&gt;![](https://pic1.zhimg.com/80/v2-9567684cec68d1618b7afac056c812a0_hd.jpg)  
马老板脸僵了五秒钟，随即绽开了笑容，说了一句：**也蛮好…**&lt;/span&gt;&lt;/div&gt;

## 关于证书：

[https://blog.csdn.net/chlinwei/article/details/67632639](https://blog.csdn.net/chlinwei/article/details/67632639)

一个数字证书= （内容 + 公钥P1（用于签名）+ 用P1对应的私钥加密过的Sig）+（CA使用其私钥S2对上面内容的签名加密后的结果 + CA公钥）

如何验证这个公钥呢？再上级的CA，直到最后根证书，是保存在浏览器中的（假装没有被篡改的可能）

虽然HTTPS主要的作用是对传输数据在信道中进行加密，但是同样有对访问网站的甄别的作用。

通过非透明的HTTPS代理，HTTPS代理的证书（eg.Charles）很可能不是一个权威的证书，所以需要手动信任他

