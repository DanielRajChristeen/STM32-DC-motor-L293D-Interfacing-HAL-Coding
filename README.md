# 🚗 STM32 DC Motor Control using L293D (HAL Coding)

A **beginner-friendly STM32 project** demonstrating how to control a **DC motor’s direction and speed** using the **L293D motor driver** and **STM32Cube HAL**.

This repository is built for **students, freshers, and STM32 learners** who want to understand *what’s happening under the hood*, not just copy-paste code.

---

## 📌 What This Project Does

Using an STM32 microcontroller, this project allows you to:

* Rotate a DC motor **Forward**
* Rotate it **Reverse**
* **Control speed** using PWM (Pulse Width Modulation)
* Safely interface a motor using an external driver (L293D)

---

## 🧠 Why We Need L293D (Very Important)

❌ STM32 GPIO pins **cannot drive motors directly**

* GPIO = 3.3V, very low current
* Motors need higher current
* Motors produce back-EMF (can damage MCU)

✅ **L293D motor driver**

* Acts as an **H-Bridge**
* Amplifies current
* Handles motor noise
* Allows **direction + speed control**

👉 Think of STM32 as the **brain**, L293D as the **muscle**.

---

## 🧩 Components Used

| Component                   | Purpose      |
| --------------------------- | ------------ |
| STM32 MCU (ex: STM32F446RE) | Controller   |
| L293D IC                    | Motor driver |
| DC Motor                    | Actuator     |
| External Power Supply       | Motor power  |
| Jumper Wires                | Connections  |

---

## 🔌 Pin Connections (Example)

| L293D Pin | Function            | STM32 Pin             |
| --------- | ------------------- | --------------------- |
| IN1       | Direction control   | PA12 (D12)            |
| IN2       | Direction control   | PA11 (D11)            |
| EN1       | Speed control (PWM) | PA5 (D13 – Timer PWM) |
| Vcc1      | Logic power         | 5V                    |
| Vcc2      | Motor power         | External supply       |
| GND       | Ground              | Common GND            |

⚠️ **STM32 GND + L293D GND + Motor GND must be common**

---

## 📁 Project Structure

```
STM32-DC-motor-L293D-Interfacing-HAL-Coding
├── Core
│   ├── Src
│   │   └── main.c
│   └── Inc
├── Drivers
├── *.ioc
├── README.md
├── LICENSE
```

---

## ▶️ How the System Works (with Code)

### 🔁 1. Motor Direction Control (GPIO)

The direction is controlled using **two GPIO pins** connected to L293D.

| IN1  | IN2  | Motor Action |
| ---- | ---- | ------------ |
| HIGH | LOW  | Forward      |
| LOW  | HIGH | Reverse      |
| LOW  | LOW  | Stop         |

### ✅ Forward Direction

```c
HAL_GPIO_WritePin(GPIOA, GPIO_PIN_12, GPIO_PIN_SET);   // IN1 = HIGH
HAL_GPIO_WritePin(GPIOA, GPIO_PIN_11, GPIO_PIN_RESET); // IN2 = LOW
```

### 🔄 Reverse Direction

```c
HAL_GPIO_WritePin(GPIOA, GPIO_PIN_12, GPIO_PIN_RESET);
HAL_GPIO_WritePin(GPIOA, GPIO_PIN_11, GPIO_PIN_SET);
```

### ⛔ Stop Motor

```c
HAL_GPIO_WritePin(GPIOA, GPIO_PIN_12, GPIO_PIN_RESET);
HAL_GPIO_WritePin(GPIOA, GPIO_PIN_11, GPIO_PIN_RESET);
```

📌 These are **pure digital outputs** — simple and predictable.

---

## ⚙️ 2. Motor Speed Control (PWM)

The **EN1 pin** of L293D is connected to a **Timer PWM output** of STM32.

* PWM duty cycle controls speed
* Higher duty → higher speed

### ▶️ Start PWM

```c
HAL_TIM_PWM_Start(&htim1, TIM_CHANNEL_1);
```

This starts **hardware PWM generation** — CPU doesn’t need to toggle pins manually.

---

### 🐢 Slow Speed (20%)

```c
__HAL_TIM_SET_COMPARE(&htim1, TIM_CHANNEL_1, 200);
```

### ⚡ Medium Speed (50%)

```c
__HAL_TIM_SET_COMPARE(&htim1, TIM_CHANNEL_1, 500);
```

### 🚀 Full Speed (100%)

```c
__HAL_TIM_SET_COMPARE(&htim1, TIM_CHANNEL_1, 1000);
```

> Timer period = 1000 → compare value decides duty cycle.

---

## 🧩 3. Complete Working Example

```c
// Set motor forward
HAL_GPIO_WritePin(GPIOA, GPIO_PIN_12, GPIO_PIN_SET);
HAL_GPIO_WritePin(GPIOA, GPIO_PIN_11, GPIO_PIN_RESET);

// Start PWM
HAL_TIM_PWM_Start(&htim1, TIM_CHANNEL_1);

// Set speed (60%)
__HAL_TIM_SET_COMPARE(&htim1, TIM_CHANNEL_1, 600);

HAL_Delay(3000);

// Stop motor
HAL_GPIO_WritePin(GPIOA, GPIO_PIN_12, GPIO_PIN_RESET);
HAL_GPIO_WritePin(GPIOA, GPIO_PIN_11, GPIO_PIN_RESET);
```

🧠 **Execution flow**

1. Direction set
2. PWM started
3. Speed adjusted
4. Delay
5. Motor stopped

This is **real-world motor control logic**.

---

## 🔁 Internal Signal Flow (Mental Model)

```
STM32 GPIO  ──► L293D IN1/IN2 ──► Direction
STM32 PWM   ──► L293D EN1     ──► Speed
L293D       ──► DC Motor
```

---

## ⚠️ Common Beginner Mistakes

❌ Forgetting common ground
❌ Powering motor from STM32
❌ Not using PWM-enabled pin
❌ Starting with full speed instantly

Hardware doesn’t forgive shortcuts.

---

## 🎓 Learning Outcomes

By completing this project, you will understand:

* DC motor basics
* Why motor drivers are required
* GPIO output control
* PWM using STM32 timers
* HAL coding structure
* Safe embedded design

---

## 🚀 Possible Extensions

* Button-controlled speed
* UART motor commands
* Encoder-based speed feedback
* PID motor control
* Register-level implementation
* Dual motor robot car

---

## 📜 License

MIT License — free to use, modify, and distribute.

---

## 🤝 Final Note

This repo is designed to **build confidence**, not just complete a task.
If you understand this project — you’ve crossed a **real embedded milestone**.

If you want next:

* Register-level version
* Flow diagrams
* Interview explanation
* LinkedIn learning post

Just say the next move 🔥
