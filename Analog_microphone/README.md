# Analog Microphone Audio Sampling using STM32H755 Nucleo

## Overview
This project demonstrates audio signal acquisition from an analog microphone using the internal ADC1 peripheral and Timer 3 (TIM3) on the STM32H755 Nucleo board (Cortex-M7 core). Captured 12-bit audio samples are sequentially stored inside a 1024-word circular ring buffer.

---

## Hardware Requirements
- STM32H755 Nucleo Board  
- Analog Microphone Module (e.g., MAX4466 / MAX9814)  
- Jumper wires  

---

## STM32CubeMX Configuration

### System & Clock Configuration
- **HSE (High-Speed External):** Crystal/Ceramic Resonator enabled  
- **NVIC Settings:** RCC global interrupt enabled  

### TIM3 Configuration
- **Clock Source:** Internal Clock  
- **Parameter Settings:**
  - Prescaler (PSC): `799`  
  - Counter Period (ARR): `999`  
- **NVIC Settings:** TIM3 global interrupt enabled  

### ADC1 Configuration
- **Channel:** IN3 (Single-ended)  
- **Parameter Settings:**
  - Clock Prescaler: Asynchronous clock mode divided by 2  
  - Resolution: ADC 12-bit  
  - Scan Conversion Mode: Disabled  
  - Continuous Conversion Mode: Disabled  
  - External Trigger Conversion: Software Start (or triggered manually inside the timer ISR)  

---

## Pin Configuration

| Peripheral | Pin | Function | Connection |
|------------|-----|----------|------------|
| ADC1_IN3   | PA6 | Analog Input | Microphone Out / VOUT |

---

## Working Principle
Timer 3 generates a periodic update interrupt based on its prescaler and counter configurations. Inside the TIM3 Interrupt Service Routine (ISR), a software trigger starts a single 12-bit analog-to-digital conversion. Once the conversion finishes, the data is pushed into a static array tracking array indexes via a circular wrapper algorithm.

---

## Code Implementation

### Private Variables (`/* USER CODE BEGIN PV */`)
```c
#define BUF_LEN 1024

uint16_t audio_buf[BUF_LEN];
volatile uint32_t write_idx = 0;
```

### Initialization & Start (`/* USER CODE BEGIN 2 */`)
```c
HAL_TIM_Base_Start_IT(&htim3);   // Start TIM3 in Interrupt Mode
HAL_ADC_Start(&hadc1);           // Initialize ADC peripheral
```

### Timer Callback Processing (`/* USER CODE BEGIN 0 */`)
```c
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
  if (htim->Instance == TIM3)
  {
    // Trigger single-channel conversion
    HAL_ADC_Start(&hadc1);
    
    // Poll conversion with a 1ms timeout window
    if (HAL_ADC_PollForConversion(&hadc1, 1) == HAL_OK)
    {
      // Extract 12-bit sample & stream to ring buffer
      audio_buf[write_idx] = HAL_ADC_GetValue(&hadc1);
      
      // Keep pointer within circular bounds
      write_idx = (write_idx + 1) % BUF_LEN;
    }
    HAL_ADC_Stop(&hadc1);
  }
}
```

---

## Testing & Verification

1. Compile and flash the application to the STM32H755 Nucleo board.
2. Connect the analog microphone output pin to **PA6**.
3. Run an IDE Live Expressions watch expression on `audio_buf` or `write_idx`.
4. Speak into the microphone and watch the 12-bit buffer array dynamically update.
