# Visual Studio 的使用#

* 改变背景颜色: Tools -> Options -> Environment -> General -> Color Theme
* 使用 git: Team explorer > Project > Changes
* show white space : ctrl + R ctrl + W [link](https://blogs.msdn.microsoft.com/zainnab/2010/04/09/view-white-space/)

## dll 和 lib 的适用范围 ##

* .lib 是 .exe 的一部分，会增加 .exe 的大小
* .dll 不是 .exe 的一部分，可以单独修改

## debug database 

* 編譯選用 Zi 將 debug database 放在 .pdb 中
* 編譯選項 Z7 將 debug database 放在每個.obj 中, 最終放在 .lib / .dll / .exe 中 


