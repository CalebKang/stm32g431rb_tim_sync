# stm32g431rb_tim_sync

STM32G431RB에서 TIM1과 TIM8의 타이머 동기화를 테스트하기 위한 예제 프로젝트입니다.

## Overview

이 프로젝트는 TIM1을 Master Timer로 사용하고, TIM8을 TIM1의 Trigger에 의해 동작하는 Slave Timer로 설정합니다.

TIM1과 TIM8은 Center-Aligned 모드로 동작하며, 각 타이머의 카운터 값은 DAC 출력으로 확인할 수 있습니다. 또한 각 타이머의 Update Interrupt 발생 시 GPIO 핀을 짧게 토글하여 오실로스코프로 인터럽트 타이밍을 확인할 수 있습니다.

## Target

* MCU: STM32G431RBTx
* Board: STM32G431RB 기반 보드
* IDE: STM32CubeIDE
* STM32Cube FW: STM32Cube FW_G4 V1.6.3

## Features

* TIM1 Master Timer 설정
* TIM8 Slave Timer 설정
* TIM8 Gated Mode 사용
* TIM1 TRGO를 이용한 TIM8 동기화
* TIM1 / TIM8 Center-Aligned Counter 동작
* DAC1 CH1, CH2를 이용한 타이머 카운터 값 출력
* GPIO 토글을 이용한 Update Interrupt 타이밍 확인

## Timer Configuration

### TIM1

* Clock Source: Internal Clock
* Counter Mode: Center-Aligned
* Prescaler: 16999
* Period: 999
* TRGO: Enable

TIM1은 Master Timer로 동작하며, TIM8의 동작을 제어하는 Trigger 신호를 생성합니다.

### TIM8

* Clock Source: ITR0
* Slave Mode: Gated Mode
* Trigger Source: TIM1
* Counter Mode: Center-Aligned
* Prescaler: 16999
* Period: 999

TIM8은 TIM1의 Trigger 상태에 따라 카운터 동작이 제어됩니다.

## Output Pins

| Function              | Pin  |
| --------------------- | ---- |
| DAC1 OUT1             | PA4  |
| DAC1 OUT2             | PA5  |
| TIM8 Update IRQ pulse | PC3  |
| TIM1 Update IRQ pulse | PC10 |

## How It Works

1. System Clock을 170 MHz로 설정합니다.
2. GPIO, TIM1, TIM8, DAC1을 초기화합니다.
3. DAC1 CH1, CH2 출력을 시작합니다.
4. TIM1과 TIM8의 Update Interrupt를 활성화합니다.
5. TIM8 카운터를 먼저 활성화합니다.
6. 약간의 지연 후 TIM1 카운터를 활성화합니다.
7. Main loop에서 TIM1 / TIM8 카운터 값을 DAC 출력으로 계속 갱신합니다.
8. 각 타이머의 Update Interrupt 발생 시 GPIO 핀을 토글합니다.

## Build & Run

1. STM32CubeIDE에서 프로젝트를 Import합니다.
2. 프로젝트를 Build합니다.
3. STM32G431RB 보드에 다운로드합니다.
4. 오실로스코프로 DAC 출력 및 GPIO 토글 핀을 확인합니다.

## Notes

* DAC 출력은 타이머 카운터 값을 아날로그 전압으로 관찰하기 위한 용도입니다.
* PC3, PC10 GPIO 출력은 타이머 Update Interrupt 타이밍 확인용입니다.
* 디버깅 중 타이머 정지를 위해 TIM1, TIM8 Debug Freeze 설정이 포함되어 있습니다.

## Project Structure

```text
Core/
  Inc/
  Src/
Drivers/
STM32G431RBTX_FLASH.ld
stm32g431rb_tim_sync.ioc
```

## License

This project is generated from STM32CubeMX / STM32CubeIDE template code.
STMicroelectronics generated source files follow the license notice included in each source file.
