````markdown
# Sine Wave Generation using STM32H755 Nucleo (DAC - M7 Core)

## Overview
This project demonstrates sine wave generation using the DAC peripheral of the STM32H755 Nucleo board (Cortex-M7 core). A lookup table containing sampled sine values is used to generate an analog waveform.

---

## Hardware Requirements
- STM32H755 Nucleo Board  
- Oscilloscope (DSO)  
- Jumper wires  

---

## STM32CubeMX Configuration

### DAC Configuration
- Enable DAC1  
- Channel: OUT1  
- Trigger: None  
- Output Buffer: Enabled  
- External Pin: Enabled (PA4)  

### GPIO Configuration
- GPIOA clock enabled  
- PA4 configured as DAC_OUT1  

### NVIC Settings
- External interrupt enabled (if required)

### Core
- Cortex-M7 core used  

---

## Pin Configuration

| Peripheral | Pin | Function |
|------------|-----|----------|
| DAC1 OUT1  | PA4 | Analog Output |

---

## Working Principle
A 256-point sine lookup table is used. The DAC outputs each value sequentially in a loop, producing a continuous sine waveform. When the index overflows, the waveform repeats.

---

## Code

```c
uint8_t lookup[256] = { /* sine values */ };
uint8_t pos = 0;

HAL_DAC_Start(&hdac1, DAC_CHANNEL_1);

while (1)
{
    HAL_DAC_SetValue(&hdac1, DAC_CHANNEL_1, DAC_ALIGN_8B_R, lookup[pos]);
    pos++;
}
````

---

## Output

* Waveform: Sine Wave
* Resolution: 8-bit
* Samples: 256 per cycle

---

## Notes

* No delay → very high frequency output
* Frequency depends on loop execution speed
* For controlled frequency:

  * Use Timer (TIM6/TIM7)
  * Use DMA (recommended)

---

## Testing

1. Flash the code
2. Connect oscilloscope to PA4
3. Observe sine waveform

---

## Conclusion

This project demonstrates basic sine wave generation using DAC and lookup table method on STM32H755 (M7 core).

```
```
