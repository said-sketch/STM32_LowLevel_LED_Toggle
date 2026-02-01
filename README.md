# STM32 Low-Level LED Toggle

## Brief Description

This project demonstrates low-level (register-level) GPIO control on an Nucleo STM32F411RE microcontroller. The LED is toggled by directly accessing hardware registers without using HAL or high-level libraries. The goal is to understand memory-mapped I/O, RCC clock control, GPIO configuration, and bitwise operations in bare-metal embedded C.

To do this low-level (LL) approach effectively, you **must understand bitwise operations** — the most important ones are:

- **SET**: set a bit to 1  ex : #define SET_BIT(reg, bit) ((reg) |= (1U << (bit)))
- **CLEAR**: set a bit to 0  ex : #define CLEAR_BIT(reg, bit) ((reg) &= ~(1U << (bit))) 
- **TOGGLE/XOR**: invert a bit  : #define TOGGLE_BIT(reg, bit) ((reg) ^= (1U << (bit))) 
- **TEST/AND**: check a bit value : #define TEST_BIT(reg, bit) (((reg) & (1U << (bit))) != 0)

Bitwise operations are the backbone of register-level programming.

---

## How to do this project

### 1 Understand the MCU structure

Before writing code, study the **block diagram of your STM32 MCU**.  
- Know which registers control which peripherals  
- Understand GPIO port mapping  
- Understand RCC (Reset and Clock Control)

---

### 2 Use the Reference Manual (RM)

The RM contains all the information you need to manipulate registers.  
For this project, we only use the registers for **clock enabling, GPIO mode, and GPIO output**. Here are the screenshots from the RM along with explanations:

#### a) RCC register for enabling GPIO clocks Example

This register controls the clocks for all GPIO ports. You must **enable the clock for GPIOA** before using it.

![RCC Enable GPIOA](images/rccenabel.PNG)

- **Explanation**: The highlighted bit in the register corresponds to GPIOA. Writing `1` to this bit enables the GPIOA peripheral clock.  
- **Code**:

```c
RCC->AHB1ENR |= (1 << 0);  // Enable GPIOA clock

> Remember: Low-level programming is all about **reading the manual, understanding the registers, and controlling bits**. Once you master this, you can program any peripheral without relying on HAL or high-level libraries.
