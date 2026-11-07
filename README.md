# 电控组第二次培训作业
- **李佳家**
- ***20263003026***

## 思路
设置PA012三个引脚 通过HAL_GPIO_WritePin控制三个引脚 使用HAL_Delay(500) 500ms切换效果

非阻塞：HAL_GetTick()返回上电以来的毫秒数 然后判断是否需要改变流水灯状态 到500ms的话就熄灭所有灯然后点亮下一个

## 代码
#define LED_NUM         3U     
#define LED_DELAY_MS    500U  
#define LED_ACTIVE_LOW  1  
/* USER CODE END PD */

/* USER CODE BEGIN PV */
static const uint16_t      led_pin [LED_NUM] = { LED0_Pin,       LED1_Pin,       LED2_Pin       };
static GPIO_TypeDef *const led_port[LED_NUM] = { LED0_GPIO_Port, LED1_GPIO_Port, LED2_GPIO_Port };

static uint32_t led_last_tick = 0U;
static uint8_t  led_index     = 0U;
/* USER CODE END PV */

/* USER CODE BEGIN 2 */
Led_AllOff();

Led_On(0);                 
led_index     = 1U;       
led_last_tick = HAL_GetTick();
/* USER CODE END 2 */

/* USER CODE BEGIN 3 */
for (uint8_t i = 0U; i < LED_NUM; i++)
{

      if ((HAL_GetTick() - led_last_tick) >= LED_DELAY_MS)
      {
        led_last_tick += LED_DELAY_MS;

        Led_AllOff();
        Led_On(led_index);
        led_index = (uint8_t)((led_index + 1U) % LED_NUM);   /* 0→1→2→0 */
      }

    }
}
/* USER CODE END 3 */
