# mybin
 flash customize bin

 通过函数spi_flash_read和spi_flash_write对闪存进行操作；
 通过闪存上传工具将数据写入闪存中。
 
 
 
## 说明
 
### 16Mbit-C1(1024+1024 map)Flash结构
https://www.espressif.com/sites/default/files/documentation/esp8266_quick_start_guide_cn.pdf

BIN|烧录地址|说明
------------------|---------------|-------------------------------
boot.bin|0x00000|主程序，位于 \bin\at
user1.2048.new.5.bin|0x01000|主程序，位于 \bin\at\1024+1024。
blank.bin|0xFE000|初始化用户参数区。
blank.bin|0x1FB000|初始化RF_CAL 参数区。
esp_init_data_default.bin|0x1FC000|初始化其他射频参数区，至少烧录一次。当 RF_CAL 参数区初始化烧录时，本区域也需烧录。
blank.bin|0x1FE000|初始化系统参数区。

(ESP8266 flash布局)[https://idarc.cn/index.php/archives/1156/]
在ESP8266_NONOS_SDK/ld/eagle.app.v6.ld中的MEMORY字段.

eagle.flash.bin和eagle.irom0text.bin的构成
eagle.flash.bin是用于存储直接读取到程序内存运行的程序段和数据内存的数据段的.
eagle.irom0text.bin是用于存储放在flash上, 不直接读取到内存中的程序段的.

程序内存:iram, internal ram, 通过ibus访问, 共32KB, 存放编译后的elf的TEXT字段
数据内存:dram, data ram, 通过 dbus访问, 可能是80kb或者96kb. 存放编译后的elf的data, rodata字段, 这部分字段可用约50kB. 其他的空间用于存放BBS, stack, heap.
FLASH存储: irom, 最大768KB, 存放编译后的elf的irom0.text字段, C源码函数前面有ICACHE_FLASH_ATTR前缀的就是说这个函数是放在irom中的. 另, 最大768KB的原因是, irom起始于256KB处, esp8266最大访问1MB程序段, 故1MB - 256KB = 768KB.
具体需要看(sdk中的eagle.app.v6.ld文件)

```
MEMORY
{
  dport0_0_seg :                        org = 0x3FF00000, len = 0x10
  dram0_0_seg :                         org = 0x3FFE8000, len = 0x14000
  iram1_0_seg :                         org = 0x40100000, len = 0x8000
  irom0_0_seg :                         org = 0x40210000, len = 0x5C000
}
```


