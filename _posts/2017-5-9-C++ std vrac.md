# std::cout

* 显示 double 类型变量，小数点后n 位。     std::fixed << setprecision (1)

      std::cout << std::fixed << setprecision (1) << 8.0 << std::endl;
      
  std::fixed 浮点数小数点后位数固定
  
* std::remove 文件名 删除文件
* std::chrono::duration 可以使用带单位的 duration

      void blink_led(milliseconds time_to_blink) // good -- the unit is explicit
      {
          // ...
          // do something with time_to_blink
          // ...
      }

      void use()
      {
          blink_led(1500ms);
      }
      
接受任何 duration 单位的函数：

      template<class rep, class period>
      void blink_led(duration<rep, period> time_to_blink) // good -- accepts any unit
      {
          // assuming that millisecond is the smallest relevant unit
          auto milliseconds_to_blink = duration_cast<milliseconds>(time_to_blink);
          // ...
          // do something with milliseconds_to_blink
          // ...
      }

      void use()
      {
          blink_led(2s);
          blink_led(1500ms);
      }
