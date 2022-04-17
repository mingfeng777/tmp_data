重要觀念 ethernet 詳解 MAC MII PHY 及register規格作用  
https://www.twblogs.net/a/5b7a98562b7177392c9666f8  
流量控制-全雙工pause (半雙工Back Pressure)  
https://blog.csdn.net/sunjice/article/details/115532563  
  
  
MII/RMII用於傳輸以太網包，在MII/RMII接口是4/2bit的，在以太網的PHY裏需要做串並轉換，  
編解碼等才能在雙絞線和光纖上進行傳輸，其幀格式遵循IEEE 802.3(10M)/IEEE 802.3u(100M)/IEEE 802.1q(VLAN)。  
以太網幀的格式爲：前導符+開始位+目的mac地址+源mac地址+類型/長度+數據+padding(optional)+32bitCRC。  
如果有vlan,則要在類型/長度後面加上2個字節的vlan tag，其中12bit來表示vlan id,另外4bit表示數據的優先級！  
  
  
重要說明  
https://zhuanlan.zhihu.com/p/340013316  
  
7.2以太網自動協商原理  
对于使用双绞线连接的以太网，如果没有数据传输时，链路并不是一直空闲，而是每隔16ms发送一个高脉冲，用来维护链路层的连接，这种脉冲成为NLP（Normal Link Pulse）码流。  
在NLP码流中再插入一些频率更高的脉冲，可用来传递更多的信息，这串脉冲成为FLP（Fast Link Pulse）码流，如下图1所示。自协商功能的基本机制就是将协商信息封装进FLP码流中，以达到自协商的目的。  
  
IEEE 802.3规范要求在下列任一情况下启动自协商：  
链路中断后恢复；设备重新上电；任何一端设备复位；有重新自协商（Renegotiation）请求。  
  
不同的frame，用source MAC的2bytes判斷(不知道VLAN位置有沒有影響)  
https://www.itread01.com/content/1550422098.html  
  
  
其他  
MAC/PHY/MII/RMII/GMII/RGMII基本介紹  
https://www.twblogs.net/a/5cabcc54bd9eee5b1a07c83e  
DM9161(phy)的datasheet  
https://pdf.ic37.com/DAVICOM_CN/DM916_datasheet_8113561/#view  
PHY Transceiver硬件設計注意事項  
https://www.twblogs.net/a/5cb3c3ffbd9eee480f07ad55  
硬件測試指南  
https://www.twblogs.net/a/5cb8fdf1bd9eee0eff45cd37  
  
  
FPGA實作簡單ethernet  
https://blog.csdn.net/qimoDIY/article/details/99519624  
  
-------------------------------------------------------------------------------------------
switch vlan  
https://blog.csdn.net/lailaiquququ11/article/details/119790507  
