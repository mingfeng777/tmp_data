Linker and Loader:  
http://wen00072.github.io/blog/2014/03/14/study-on-the-linker-script/


CPSR:  
CPSR暫存器和另外5個SPSR暫存器，主要功能是記錄最近的ALU運算狀態，控制中斷的致能﹙enable interrupt﹚與禁止﹙disable interrupt﹚。CPSR的4個運算結果旗標﹙flag﹚：N﹙Negative﹚、Z﹙Zero﹚ 、C﹙Carry﹚和V﹙Overflow﹚可以表式最近執行過的算術指令的狀態。此4位元使用了CPSR[28:34]。CPSR[8:27]為保留，留待ARM未來定義。CPSR[7:7]為將IRQ ﹙interrupt request﹚設為致能/禁止的位元。要控制Fiq ﹙fast interrupt﹚設為致能/禁止，則設定CPSR[6:6]的位元。CPSR[5:5]state bit  
CPSR[4:0]共5個位元，用來選擇一個前述ARM CPU的模式。當CPSR[4:0]=10000B時，是為用戶模式。在Fiq模式時，CPSR[4:0]=10001B。若要為IRQ模式，則CPSR[4:0]=10010。當在監督模式﹙supervisor mode﹚時，CPSR[4:0]=10011B。而在中止模式﹙abort mode﹚時，CPSR[4:0]=10111B。在未定義模式﹙undefined mode﹚下，CPSR[4:0]=11011B，在系統模式﹙system mode﹚則CPSR[4:0]=11111B  
CPSR[4:0]|模式﹙mode﹚  
10000    |用戶模式﹙User﹚  
10001    |快速中斷模式﹙FIQ﹚  
10010    |中斷模式﹙IRQ﹚  
10011    |監督模式﹙Supervisor﹚  
10111    |中止模式﹙Abort﹚  
11011    |未定義模式﹙Undefined﹚  
11111    |系統模式h﹙System﹚  
