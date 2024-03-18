- GPIOB->BSRR = 0x01就是把GPIOB port 0升为高电平
- GPIOB->BRR = 0x01就是把GPIOB port 0降为低电平
- GPIOB->BSRR = 0x02就是把GPIOB port 1升为高电平
- GPIOB->BRR = 0x02就是把GPIOB port 1降为低电平
- GPIOB->BSRR = 0x04就是把GPIOB port 2升为高电平
- GPIOB->BRR = 0x04就是把GPIOB port 2降为低电平
- GPIOB->BSRR = 0x08就是把GPIOB port 3升为高电平
- GPIOB->BRR = 0x08就是把GPIOB port 3降为低电平

BSRR高16位用于清0 低16位用于设置

BRR低16位用于清0 高16位保持