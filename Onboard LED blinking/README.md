# STM32H755ZIQ Nucleo Triple LED Blinky (M7 Core)

This project implements a sequential triple-LED blinking application running exclusively on the high-performance **Cortex-M7 (CM7)** core of the **NUCLEO-H755ZI-Q** dual-core development board. It utilizes the STMicroelectronics HAL drivers for peripheral control.

## 🚀 Features
* **Active M7 Core Execution**: Runs entirely on the primary Cortex-M7 core.
* **Sequential Blinking**: Toggles three onboard LEDs in sequence with a 1-second delay between transitions.
* **Internal Clock Configuration**: Uses the High-Speed Internal (HSI) oscillator configured via STM32CubeMX HAL architecture.

## 📌 Hardware Pin Mapping
The application sequences through the three user LEDs available on the Nucleo-144 board:

| LED Color | GPIO Port | Pin Number | State Sequence |
| :--- | :--- | :--- | :--- |
| **Yellow** | `GPIOE` | `GPIO_PIN_1` | First |
| **Green** | `GPIOB` | `GPIO_PIN_0` | Second |
| **Red** | `GPIOB` | `GPIO_PIN_14` | Third |

## ⚠️ Core Configuration Note
* **Single-Core Operation**: Lines 35 to 45 in the source file (`DUAL_CORE_BOOT_SYNC_SEQUENCE` macro and its hardware semaphore definitions) are explicitly **commented out**. 
* **Impact**: Because these lines are disabled, the program executes standalone on the **Cortex-M7** core for straightforward debugging. It completely bypasses the hardware synchronization handshake that would normally wake up the secondary Cortex-M4 core. 

## 🛠️ Project Structure
* **`main.c`**: Contains the application initialization code, system clock configuration, GPIO setup, and the infinite execution loop (`while(1)`).
* *Note: This file depends on a valid local `main.h` configuration header.*

## 💻 Code Logic Overview
The infinite loop controls the hardware pins sequentially using standard HAL API blocks:
```c
// Example snippet from the main loop
HAL_GPIO_WritePin(GPIOE, GPIO_PIN_1, SET);
HAL_Delay(1000);
HAL_GPIO_WritePin(GPIOE, GPIO_PIN_1, RESET);
HAL_Delay(1000);
```

## ⚙️ How to Run
1. Open your workspace using **STM32CubeIDE**.
2. Make sure this code replaces the default `main.c` file within your project's **`CM7/Core/Src/`** folder.
3. Build the project to verify that `main.h` dependencies resolve properly.
4. Connect the NUCLEO-H755ZI-Q board via Micro-USB and flash the binary to the board.
