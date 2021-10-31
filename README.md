格式救星:  
https://www.itread01.com/content/1546357890.html  
-------------------------------------------------------------------------------------------------  
Linker and Loader:  
http://wen00072.github.io/blog/2014/03/14/study-on-the-linker-script/  
  
uboot if_changed:  
The rule is in scripts/Kbuild.include.  
old version say the following,  
`# Find any prerequisites that are newer than target or that do not exist.`  
`# (This is not true for now; $? should contain any non-existent prerequisites,`  
`# but it does not work as expected when .SECONDARY is present. This seems a bug`  
`# of GNU Make.)`  
`# PHONY targets skipped in both cases.`  
`newer-prereqs = $(filter-out $(PHONY),$?)`  
new version change the following,  
`# Find any prerequisites that is newer than target or that does not exist.`  
`# PHONY targets skipped in both cases.`  
`any-prereq = $(filter-out $(PHONY),$?) $(filter-out $(PHONY) $(wildcard $^),$^)`  
https://www.twblogs.net/a/5b85a9142b71775d1cd391f1  
  
uboot SPL理解(以下說明主要是ARM在嵌入式系統上)  
flash分為nor flash和nand flash。nor flash可直接運行程式(原地執行,XIP)，nand flash不行。  
要讓nand flash裡的程式運行，已知方法是將程式(bootloader)載入到SoC的SRAM，程式可運行在SRAM上，但bootloader大小有時會大於SRAM，因此步驟變成兩個stage。  
SPL全名是Secondary Program Loader，當bootloader大小大於SRAM時，bootloader就要分兩階段。  
第一階段:開機時硬體自動的把flash上的資料(不會超過SRAM size)載入到SRAM上，並開始執行。硬體設備初始化，RAM(SDRAM)初始化，設置CPU模式。再將bootloader載入到SDRAM(包含了第二階段的進入點)，再跳到第二階段的進入點  
第二階段:初始化其他的硬體設備，檢查memory mapping，設定啟動參數...等  
第一階段會做比較精簡有力的動作(運行的CODE主要是start.S相關，用組語寫是最精簡的)，第二階段會用C做較複雜的動作。  
已知s3c2440使用4K的SRAM並將此協助nand flash開機動作稱為stepping stone，s3c2440如果是nand flash開機，初始位置0x00000000是在SRAM上(ARM初始運行位置是在0x00000000，MIPS則是0xBFC00000)  
http://albert-oma.blogspot.com/2016/07/embedded-u-boot.html  
https://www.gushiciku.cn/pl/pspZ/zh-tw  

CPSR:  
CPSR暫存器和另外5個SPSR暫存器，主要功能是記錄最近的ALU運算狀態，控制中斷的致能﹙enable interrupt﹚與禁止﹙disable interrupt﹚。CPSR的4個運算結果旗標﹙flag﹚：N﹙Negative﹚、Z﹙Zero﹚ 、C﹙Carry﹚和V﹙Overflow﹚可以表式最近執行過的算術指令的狀態。此4位元使用了CPSR[28:34]。CPSR[8:27]為保留，留待ARM未來定義。CPSR[7:7]為將IRQ ﹙interrupt request﹚設為致能/禁止的位元。要控制Fiq ﹙fast interrupt﹚設為致能/禁止，則設定CPSR[6:6]的位元。CPSR[5:5]State Bit或Thumb Bit，T=0 指示ARM執行。T=1指示Thumb執行。  
CPSR[4:0]共5個位元，用來選擇一個前述ARM CPU的模式。當CPSR[4:0]=10000B時，是為用戶模式。在Fiq模式時，CPSR[4:0]=10001B。若要為IRQ模式，則CPSR[4:0]=10010。當在監督模式﹙supervisor mode﹚時，CPSR[4:0]=10011B。而在中止模式﹙abort mode﹚時，CPSR[4:0]=10111B。在未定義模式﹙undefined mode﹚下，CPSR[4:0]=11011B，在系統模式﹙system mode﹚則CPSR[4:0]=11111B  
CPSR[4:0]|模式﹙mode﹚  
10000    |用戶模式﹙User﹚  
10001    |快速中斷模式﹙FIQ﹚  
10010    |中斷模式﹙IRQ﹚  
10011    |監督模式﹙Supervisor﹚  
10111    |中止模式﹙Abort﹚  
11011    |未定義模式﹙Undefined﹚  
11111    |系統模式h﹙System﹚  
http://stenlyho.blogspot.com/2008/08/arm-register.html  
  
perf: (perf sched)  
https://jasonblog.github.io/note/linux_tools/perf_--_linuxxia_de_xi_tong_xing_neng_diao_you_gon.html
  
ex.抓取特定CPU上的延遲
perf record -e sched:sched_wakeup,sched:sched_wakeup_new,sched:sched_switch -C 93,189 -- sleep 1
  
debug packet lost in kernel:  
1.dropwatch---------->https://blog.huoding.com/2016/12/15/574 source code:https://github.com/nhorman/dropwatch  
2.perf monitor kfree_skb event--->perf record -g -a -e skb:kfree_skb; perf script  
3.tcpdrop  
4.systemtap script  
  
Makefile:  
https://www.cnblogs.com/shiwenjie/category/981510.html  
  
https://leetcode.com/problems/maximum-product-of-the-length-of-two-palindromic-subsequences/discuss/1458289/Mask-DP-4-ms  
先尋找字串中的最大迴文數，暴力方法遍歷所有可能並存在陣列中(用空間暫時儲存結果，DP)。  
遍歷方法是判斷字元出現與否(1bit)，字串字元都出現就是都是1，可轉成數字表示  
例:  字串abcd，每個字元可出現或沒出現，出現者為1，相反則為0，出現abd->1101(二進位表示)->13(十進位)。因此所有可能就是0~15(或16-1)  
第二段的子遮罩，https://cp-algorithms.com/algebra/all-submasks.html  
但其實最後的迴圈還是會有重複性，例如:mask是110000，連續0就會有重複的多餘動作  


-------------------------------------------------------------------------------------------------  
USB debug need enable  
->"CONFIG_USB_MON"  
location is  
->"driver/usb/mon/"  
description file  
->"Documentation/usb/usbmon.txt"  
http://www.usbzh.com/article/detail-330.html  
  
hub driver  
https://www.cxybb.com/article/lujian186/82085535  
  
-------------------------------------------------------------------------------------------------  
debug netfilter  
/net/netfilter/core.c  
https://blog.csdn.net/xiaoyu_750516366/article/details/88775990  
CONFIG_NETFILTER_DEBUG 好像有些rule會直接寫回傳值(okfn)，或搜尋完畢回傳預設值，開啟這個才會讓所有封包都進nf_hook_slow  
important function "nf_hook_slow"  
https://www.cnblogs.com/haoqingchuan/p/6240483.html  
https://www.cnblogs.com/wanpengcoder/p/11755574.html  
https://zhuanlan.zhihu.com/p/322311354  
netfilter hook example  
http://neokentblog.blogspot.com/2014/06/netfilter-hook.html  
default packet flow  
https://lyt0112.pixnet.net/blog/post/305966017-linux%E5%85%A7%E6%A0%B8%E4%B8%ADnetfilter-hook%E7%9A%84%E4%BD%BF%E7%94%A8  
other  
重要流程圖  
http://bbs.chinaunix.net/thread-3749229-1-1.html
  
-------------------------------------------------------------------------------------------------  
樹梅派4的OpenWrt 選項  
https://ithelp.ithome.com.tw/articles/10255724?sc=rss.qu  
openwrt github  
https://github.com/openwrt/openwrt  
OpenWRT關於樹梅派數據  
https://openwrt.org/toh/raspberry_pi_foundation/raspberry_pi  
