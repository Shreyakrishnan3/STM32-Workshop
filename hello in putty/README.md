# 📡 STM32 USART3 "Hello" Output to PuTTY

## 🧾 Overview
This project demonstrates how to transmit a simple `"Hello"` message from an STM32H755ZI (NUCLEO board) to a PC using **USART3**, and display it in **PuTTY** via serial communication.

---

## ⚙️ Hardware Requirements
- STM32H755ZI NUCLEO Board  
- USB Cable (ST-Link Virtual COM Port)  
- PC/Laptop  

---

## 🧰 Software Requirements
- STM32CubeIDE  
- PuTTY  

---

## 🧩 STM32CubeIDE Setup

### 1️⃣ Create New Project
- Open **STM32CubeIDE**
- Go to: `File → New → STM32 Project`
- Select MCU: `STM32H755ZIT6`

---

### 2️⃣ Configure USART3
- Navigate to: **Connectivity → USART3**
- Set Mode: **Asynchronous**
- Configure Pins:
  - `PD8 → TX`
  - `PD9 → RX`

---

### 3️⃣ UART Parameters
| Parameter            | Value   |
|---------------------|--------|
| Baud Rate           | 115200 |
| Word Length         | 8 Bits |
| Stop Bits           | 1      |
| Parity              | None   |
| Flow Control        | None   |

---

### 4️⃣ Enable Interrupt (Optional)
- Go to **NVIC Settings**
- Enable: `USART3 global interrupt`

---

### 5️⃣ Clock Configuration
- Open **Clock Configuration Tab**
- Resolve any warnings/errors
- Ensure USART clock source is valid

---

## 💻 Code

Add the following inside the `while(1)` loop in `main.c`:

```c
char msg[] = "Hello\r\n";

HAL_UART_Transmit(&huart3,
                  (uint8_t *)msg,
                  strlen(msg),
                  HAL_MAX_DELAY);

HAL_Delay(1000);
```

---

## 🔌 Find COM Port

1. Open **Device Manager**
2. Expand **Ports (COM & LPT)**
3. Note:
   ```
   STMicroelectronics STLink Virtual COM Port (COMx)
   ```

---

## 🖥️ PuTTY Configuration

1. Open **PuTTY**
2. Select **Serial**
3. Set:
   - Serial Line: `COMx`
   - Speed: `115200`
4. Click **Open**

---
## click resolve clock configurations button

## ✅ Expected Output

```
Hello
Hello
Hello
...
```

(Printed every 1 second)

---

## ❗ Troubleshooting

### 🔴 No Output
- Check correct COM port
- Ensure board is powered
- Verify USART3 pin configuration (PD8/PD9)

---

### 🔴 Garbage Output
- Baud rate mismatch → ensure **115200** in both STM32 & PuTTY

---

### 🔴 COM Port Not Visible
- Check USB cable
- Install/update ST-Link drivers
- Try different USB port

---

### 🔴 Code Not Running
- Ensure:
  - `HAL_Init()` is called
  - `SystemClock_Config()` is correct
- Rebuild & flash again

---

## 🎯 Concepts Covered
- UART Communication (USART3)
- STM32 HAL API
- Serial Terminal Interfacing
- Embedded Debugging Basics

---

## 🚀 Next Steps
- Receive data from PuTTY (UART RX)
- Use Interrupt-based UART
- Implement DMA-based UART

---
