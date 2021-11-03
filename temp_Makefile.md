  Makefile:
  https://www.cnblogs.com/shiwenjie/category/981510.html

  -------
  %:: 和 %: 區別單冒號和雙冒號
  當同一個檔案作為多個雙冒號規則的目標時。這些不同的規則會被獨立的處理,而不是像普通規則那樣合併所有的依賴到一個目標檔案
  https://www.cnblogs.com/zxc2man/p/3759770.html
  https://www.itread01.com/p/194699.html

  -------
  指令前的+號
  Makefile有些選項-n,-t,-s可用來查閱運行過程，有些運行過程有依賴關係無法得知真正流程，所以還有更新用的選項
  指令前的'+'號使該指令不管那三個選項。
  文件中寫道:
  標誌'-n','-t'和'-s'對那些字元'+'開始的命令行和包含字串'$(MAKE)'或'${MAKE}'命令行不起作用
  9.3代替執行命令

  -------
  eval & call
  
  https://www.cnblogs.com/merlindu/p/6542805.html
  https://www.twblogs.net/a/5b8b18232b717718832d46f9
