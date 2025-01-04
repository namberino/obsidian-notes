PWM is 0 - 3.3V

PWM has 2 things:
- Duty Cycle
- Frequency

Duty cycle:
- The duty cycle is the percentage of time for which the PWM signal stays HIGH or is "on time."
- Example: A signal that is always OFF has a 0% duty cycle, whereas a signal that is constantly ON has a 100% duty cycle
- **Duty Cycle = ON Time of Signal / Time Period of Signal**
![[esp-idf-pwm-duty-cycle.png]]

Frequency:
- The frequency of a PWM signal is the number of cycles per second
- It also defines how quickly a PWM signal completes one cycle which is known as the time period
- Example: If a signal has a 20ms time period, then it will have a frequency of 50Hz
- **Frequency = 1/Time Period**
- **Time Period = ON time + OFF time**

PWM Supported Pins:
- GPIO0-GPIO5  
- GPIO12-GPIO33

Library:
```c
#include "driver/ledc.h"
```

- This library is used to control LEDs and produce PWM signals
- The 16 LEDC channels are divided into two groups of 8 channels that give rise to separate waveforms. 
- Out of the two groups of LEDC channels, one works in high speed and the other works in low-speed mode

Timer config:
- Configure the timer by defining the PWM signal’s duty cycle resolution, frequency, speed mode, timer number, and source clock
- Use `ledc_timer_config()`. Parameters:
	- Speed mode (`ledc_mode_t`)
	- PWM signal frequency
	- Resolution of duty cycle
	- Timer number (`ledc_timer_t`)
	- Source Clock (`ledc_clk_cfg_t`)

> **Note**: The duty cycle resolution and source clock are related to the frequency of the PWM signal. 
> - The duty cycle resolution has an inverse relationship with the PWM frequency (The higher the PWM frequency, the lower is the duty cycle resolution and vice versa)
> - The source clock frequency has a direct relationship with the PWM signal frequency (The higher the source clock frequency, the higher the maximum PWM frequency can be set and vice versa)

```c
ledc_timer_config(const ledc_timer_config_t *timer_conf)
```


Clock sources:

|Clock Type|Clock Frequency|Speed Mode|
|---|---|---|
|APB_CLK|80MHz|High or Low|
|REF_TICK|1MHz|High or Low|
|RTC8M_CLK|8MHz|Low|

The `REF_TICK` clock is capable of dynamic frequency scaling whereas the `RTC8M_CLK `clock is capable of both dynamic frequency scaling and light sleep


Channel config:
- This function takes in the `ledc_channel_config_t` structure that defines the channel configuration parameters inside it
- This will enable the PWM signal to be generated at the specified GPIO along with the frequency and duty cycle resolution already set in the timer configuration
- Call the `ledc_stop()` to stop PWM generation
```c
ledc_channel_config(const ledc_channel_config_t *ledc_conf)
```

Duty cycle:
- Changing the PWM signal means varying the duty cycle so the brightness of the LED changes accordingly
```c
ledc_set_duty(ledc_mode_t speed_mode, ledc_channel_t channel, uint32_t duty)
```
- 1st parameter is the speed mode
- 2nd parameter is the LEDC channel
- 3rd parameter is the LEDC duty that you want to set

- `ledc_update_duty()` is used to update the LEDC channel parameters
```c
ledc_update_duty(ledc_mode_t speed_mode, ledc_channel_t channel)
```

> **Note**: After calling `ledc_set_duty()`, it is necessary to call the `ledc_update_duty()` function as it is responsible for activating the LEDC duty changes that were set

LED potentiometer control:
- Control the brightness of the LED connected at GPIO27 through an average of the raw ADC values acquired from ADC1 Channel 4
```c
#include <stdio.h>
#include <stdlib.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "driver/adc.h"
#include "driver/ledc.h"

#define SAMPLE_CNT 32
static const adc1_channel_t adc_channel = ADC_CHANNEL_4;
#define LEDC_GPIO 27
static ledc_channel_config_t ledc_channel;

static void init_hw(void)
{
    adc1_config_width(ADC_WIDTH_BIT_10);
    adc1_config_channel_atten(adc_channel, ADC_ATTEN_DB_11);
    ledc_timer_config_t ledc_timer = {
        .duty_resolution = LEDC_TIMER_10_BIT,
        .freq_hz = 1000,
        .speed_mode = LEDC_HIGH_SPEED_MODE,
        .timer_num = LEDC_TIMER_0,
        .clk_cfg = LEDC_AUTO_CLK,
    };

    ledc_timer_config(&ledc_timer);
    ledc_channel.channel = LEDC_CHANNEL_0;
    ledc_channel.duty = 0;
    ledc_channel.gpio_num = LEDC_GPIO;
    ledc_channel.speed_mode = LEDC_HIGH_SPEED_MODE;
    ledc_channel.hpoint = 0;
    ledc_channel.timer_sel = LEDC_TIMER_0;
    ledc_channel_config(&ledc_channel);
}

void app_main()
{
    init_hw();
    while (1)
    {
        uint32_t adc_val = 0;
        for (int i = 0; i < SAMPLE_CNT; ++i)
        {
            adc_val += adc1_get_raw(adc_channel);
        }
        adc_val /= SAMPLE_CNT;


        ledc_set_duty(ledc_channel.speed_mode, ledc_channel.channel, adc_val);
        ledc_update_duty(ledc_channel.speed_mode, ledc_channel.channel);
        vTaskDelay(500 / portTICK_RATE_MS);
    }
}
```

Explanation:
```c
#define SAMPLE_CNT 32
```
- Sample count
- Read ADC value 32 times, average them out and set it as the duty cycle

```c
static const adc1_channel_t adc_channel = ADC_CHANNEL_4;
```
- Use ADC channel 4 (potentiometer connection)

```c
#define LEDC_GPIO 27
static ledc_channel_config_t ledc_channel;
```
- LED on pin 27
- Define an LED control channel

```c
static void init_hw(void)
{
    adc1_config_width(ADC_WIDTH_BIT_10);
    adc1_config_channel_atten(adc_channel, ADC_ATTEN_DB_11);
    ledc_timer_config_t ledc_timer = {
        .duty_resolution = LEDC_TIMER_10_BIT,
        .freq_hz = 1000,
        .speed_mode = LEDC_HIGH_SPEED_MODE,
        .timer_num = LEDC_TIMER_0,
        .clk_cfg = LEDC_AUTO_CLK,
    };

    ledc_timer_config(&ledc_timer);
    ledc_channel.channel = LEDC_CHANNEL_0;
    ledc_channel.duty = 0;
    ledc_channel.gpio_num = LEDC_GPIO;
    ledc_channel.speed_mode = LEDC_HIGH_SPEED_MODE;
    ledc_channel.hpoint = 0;
    ledc_channel.timer_sel = LEDC_TIMER_0;
    ledc_channel_config(&ledc_channel);
}
```
- This initializes the hardware
- First we have set the ADC bit width for ADC1 at 10 bit width using the function adc1_config_width(). This means that the ADC values will vary between 0-1023. 
- Then, we will configure the attenuation parameter of ADC1 channel 4 to 11db. This sets the ADC input voltage range to approximately 150- 2450mV

- For initializing the LED control channel, configure the timer first:
	- Using LEDC_TIMER_0 with 1000Hz frequency
	- For the PWM duty cycle, set its resolution to 10 bits
	- Speed mode is high speed mode

- We are using LEDC_CHANNEL_0 on our LEDC_GPIO which we previously defined as GPIO27 (connected with the LED)
- You can avail up to 8 channels as they are available
- This LEDC_GPIO which we have configured uses the LEDC_TIMER_0 as previously set up

```c
void app_main()
{
    init_hw();
    while (1)
    {
        uint32_t adc_val = 0;
        for (int i = 0; i < SAMPLE_CNT; ++i)
        {
            adc_val += adc1_get_raw(adc_channel);
        }
        adc_val /= SAMPLE_CNT;

        ledc_set_duty(ledc_channel.speed_mode, ledc_channel.channel, adc_val);
        ledc_update_duty(ledc_channel.speed_mode, ledc_channel.channel);
        vTaskDelay(500 / portTICK_RATE_MS);
    }
}
```
- ADC value is sampled 32 times and averaged out to get a good approximate value
- Then we set the duty cycle with the ADC value and update the new duty cycle to the PWM channel
