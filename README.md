# mybin
 flash customize bin

 通过函数spi_flash_read和spi_flash_write对闪存进行操作；
 通过闪存上传工具将数据写入闪存中。
 
 
 
## 说明
 
### 16Mbit-C1 (1024+1024 map) Flash，结构：
BIN|烧录地址|说明
------------------|---------------|-------------------------------
blank.bin 0x1FB000|初始化|RF_CAL 参数区。
esp_init_data_default.bin|0x1FC000|初始化其他射频参数区，至少烧录一次。当 RF_CAL 参数区初始化烧录时，本区域也需烧录。
blank.bin|0xFE000|初始化⽤户参数区。
blank.bin|0x1FE000|初始化系统参数区。
boot.bin|0x00000|主程序，位于 \bin\at
user1.2048.new.5.bin|0x01000|主程序，位于 \bin\at\1024+1024。