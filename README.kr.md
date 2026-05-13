<table class="sphinxhide">
 <tr>
    <td align="center"><img src="https://raw.githubusercontent.com/Xilinx/Image-Collateral/main/xilinx-logo.png" width="30%"/><h1>AXI_1WIRE_HOST</h1>
    </td>
 </tr>
</table>

# :warning: 면책 조항

**AXI 1-Wire Host는 AMD의 보증 또는 지원 없이 무료로 제공됩니다. AXI 1-Wire Host에 대해서는 제한적인 테스트만 수행되었습니다. 설계를 테스트하고 올바르게 동작하는지 확인할 책임은 사용자에게 있습니다.**

AMD는 AXI 1-Wire Host에 대한 유지보수를 제공하지 않습니다. 버그나 문제를 발견한 경우, 향후 릴리스가 진행될 때 AMD에 알릴 수 있도록 이 리포지토리에 새 이슈를 생성하십시오.

## 소개

AMD AXI 1-Wire Host 코어는 AXI 인터페이스에 1-Wire 버스 컨트롤러 인터페이스를 제공합니다. 이 32비트 소프트 코어는 AXI4-Lite 인터페이스와 연동하도록 설계되었습니다. 이 코어는 **AMD 프로그래머블 로직 IP 코어용 AXI 1-wire host driver** Linux 드라이버와 호환됩니다.

## 기능

- AXI4-Lite 인터페이스 사양을 지원합니다.
- 1-Wire 버스 프로토콜을 지원합니다.
- 다음 1-Wire 버스 마스터 신호를 지원합니다.
  - Reset/Presence 신호
  - 쓰기 비트(0 또는 1) 신호
  - 읽기 비트(0 또는 1) 신호
- 선택적 인터럽트 요청 생성을 지원합니다.
- 1-Wire Host 코어의 소프트 리셋을 지원합니다.
- 1-Wire 버스의 비트 뱅잉을 사용하는, 구성 가능한 단일 범용 I/O(GPIO) 채널을 지원합니다.
- **AMD 프로그래머블 로직 IP 코어용 AXI 1-wire host driver** Linux 드라이버와 호환됩니다.

## 기능 설명

AXI 1-Wire Host는 AXI4-Lite 인터페이스에 1-Wire 버스 컨트롤러 인터페이스를 제공합니다. AXI 1-Wire는 필수 1-Wire 버스 마스터 신호를 지원하면서 1-Wire 버스 프로토콜을 지원합니다. AXI 1-Wire Host에는 보드 외부 장치를 구동하기 위한 프로토콜 타이밍을 보장하는 1-Wire Host Core Controller가 포함되어 있습니다. 컨트롤러가 신호 명령 사이클을 완료할 때 인터럽트를 생성하도록 구성할 수 있습니다. AXI 1-Wire Host는 1-Wire Host Core Controller를 바이패스하여 1-Wire 버스를 범용 I/O(GPIO)로 조작하도록 구성할 수 있습니다. AXI 1-Wire Host의 주요 구성 요소는 AXI4-Lite 인터페이스, 1-Wire Host Core Controller, 인터럽트 컨트롤러 및 GPIO 모듈입니다.

### AXI4-Lite 인터페이스

AXI4-Lite Interface 모듈은 1-Wire Host 및 GPIO 레지스터에 접근하기 위한 32비트 AXI4-Lite 슬레이브 인터페이스를 구현합니다. AXI4-Lite 슬레이브 인터페이스에 대한 추가 세부 정보는 *AXI4-Lite IPIF LogiCORE IP Product Guide* [(PG155)](https://docs.amd.com/access/sources/ud/document?isLatest=true&url=pg155-axi-lite-ipif&ft:locale=en-US)의 사양 사용 섹션을 참조하십시오.

### 1-Wire Host Core Controller

1-Wire Host Core Controller는 보드 외부 장치를 구동하기 위한 1-Wire 프로토콜 타이밍을 보장하는 하드웨어를 구현합니다. 외부 프로세서가 이를 제어하여 1-Wire 버스 마스터 신호를 생성하고 보고합니다. 다음 다섯 가지 명령을 지원합니다.

- Reset/Presence
- 쓰기 비트(0 또는 1)
- 읽기 비트(0 또는 1)
- 바이트 쓰기
- Ready byte.

### 인터럽트 컨트롤러

인터럽트 제어부는 1-Wire Host Core Controller에서 인터럽트 상태를 가져와 외부 프로세서로 인터럽트를 생성합니다.

GPIO 모듈은 1-Wire Host Core Controller를 바이패스하고 GPIO를 통해 1-Wire 버스를 조작하기 위한 3상태 버퍼를 구현합니다.

## 포트 설명

AXI 1-Wire HOST I/O 신호는 다음 표에 나열되어 설명되어 있습니다.
| 신호 이름 | 인터페이스 | I/O | 설명 |
|--|--|--|--|
| s00_axi_aclk| 클록 | I | AXI 클록|
| S00_axi_aresetn | 리셋 | I | AXI 리셋, Active-Low.|
| s00_axi_*| S_AXI | NA | AXI4-Lite 슬레이브 인터페이스 신호. AXI4, AXI4-Lite 및 AXI4-Stream 신호에 대해서는 *Vivado AXI Reference Guide* [(UG1037)](https://docs.amd.com/go/en-US/ug1037-vivado-axi-reference-guide)의 부록 A를 참조하십시오.|
| w1_bus | 신호 | IO | 1-Wire 버스 입출력 핀.|
| w1_irq | 인터럽트 | O | AXI 1-Wire Host 인터럽트. Active-High, 레벨 감지 신호.|

## 사용자 파라미터

AXI 1-Wire Host 사용자 파라미터는 다음 표에 나열되어 설명되어 있습니다.
| Vivado IDE 파라미터 이름 | 파라미터 이름 | 기본값 | 설명 |
|--|--|--|--|
| S00 AXI CLK DIVIDER | CLK_DIV_VAL_TO_1MHz | 100 | 1MHz 클록을 생성하기 위한 S00_AXI_CLK 분주 값입니다(예: 100MHz S00_AXI_CLK의 경우 분주 값은 100). |

## 레지스터 공간

AXI 1-Wire Host 레지스터는 다음 표에 나열되어 설명되어 있습니다.
| 주소 공간 오프셋 | 레지스터 이름 | 접근 유형 | 기본값 | 설명|
|--|--|--|--|--|
| 0x0000 | w1_INSTR| R/W | 0x0 | AXI 1-Wire Host 명령 레지스터.|
| 0x0004 | w1_CTRL | R/W | 0x0 | AXI 1-Wire Host 제어 신호 레지스터.|
| 0x0008 | w1_IRQCTRL| R/W | 0x0 | AXI 1-Wire Host 인터럽트 제어 레지스터.|
| 0x000C | w1_STAT| R/W | 0x0 | AXI 1-Wire Host 상태 신호 레지스터.|
| 0x0010 | w1_RXDATA | R/W | 0x0 | 1-Wire Host Core Controller를 통해 수신한 AXI 1-Wire Host 데이터 레지스터.|
| 0x0014 | w1_GPIODATA | R/W | 0x0 | GPIO를 통해 수신한 AXI 1-Wire Host 데이터 레지스터.|
| 0x0018 | w1_IPVER| R | 0x7600_0100 | AXI 1-Wire Host 버전 레지스터.|
| 0x001C | w1_IPID| R | 0x10EE_4453 | AXI 1-Wire Host 식별 레지스터.|

### AXI 1-Wire Host 명령 레지스터(w1_INSTR)

AXI 1-Wire Host Instruction Register는 외부 프로세서가 1-Wire Host에서 실행할 명령을 지정하는 데 사용됩니다. 이 레지스터의 기능은 다음 표에 자세히 설명되어 있습니다.
| 비트 | 필드 이름 | 설명 |
|--|--|--|
| [31] | GPIO/Controller enable | 0 = Controller 활성화/GPIO 비활성화 <br/> 1 = Controller 비활성화/GPIO 활성화 |
| [23] | GPIO Tri State | 0 = I/O 핀이 출력으로 구성되어 1-Wire 버스에 씁니다. <br/> 1 = I/O 핀이 입력으로 구성되어 1-Wire 버스에서 읽습니다.|
| [16] | GPIO Output | GPIO 출력으로 구성된 경우의 1-Wire 버스 레벨.|
| [11:8] | Controller Instruction | 1000 = Reset/Presence 펄스 <br/> 1110 = 쓰기 비트 펄스 <br/> 1100 = 읽기 비트 펄스 <br/> 1111 = 바이트 쓰기(8개의 쓰기 비트 펄스) <br/> 1101 = Ready byte(8개의 읽기 비트 펄스)|
| [7:0] | Data to write | 1-Wire 버스로 전송할 비트입니다. 컨트롤러 명령이 *Write Bit*인 경우 최하위 비트(LSB)만 전송되고, 명령이 *Write Byte*인 경우 8비트가 모두 전송됩니다. |

### AXI 1-Wire Host 제어 레지스터(w1_CTRL)

AXI 1-Wire Host Control Register는 외부 프로세서가 1-Wire Host Core Controller의 처리를 제어하는 데 사용됩니다. 이 레지스터의 기능은 다음 표에 자세히 설명되어 있습니다.
| 비트 | 필드 이름 | 설명 |
|--|--|--|
| [31] | Reset Controller | 1-Wire Host Core Controller의 소프트 리셋, Active-High. |
| [0] | GO Signal | 외부 프로세서가 1-Wire Host Core Controller에 명령을 가져와 실행할 수 있음을 알리는 데 사용됩니다. |

### AXI 1-Wire Host 인터럽트 제어 레지스터(w1_IRQCTRL)

AXI 1-Wire Host Interrupt Control Register는 외부 프로세서가 1-Wire Host Core Controller에서 생성되는 인터럽트를 마스크하는 데 사용됩니다. 이 레지스터의 기능은 다음 표에 자세히 설명되어 있습니다.
| 비트 | 필드 이름 | 설명 |
|--|--|--|
| [4] | Ready Interrupt Mask | 0: 1-Wire Host Core Controller *READY* 신호가 인터럽트를 발생시키지 않습니다. <br/> 1: 1-Wire Host Core Controller *READY* 신호가 인터럽트를 발생시킵니다. Active-High.|
| [0] | Done Interrupt Mask | 0: 1-Wire Host Core Controller *DONE* 신호가 인터럽트를 발생시키지 않습니다. <br/> 1: 1-Wire Host Core Controller *DONE* 신호가 인터럽트를 발생시킵니다. Active-High.|

### AXI 1-Wire Host 상태 레지스터(w1_STAT)

AXI 1-Wire Host Status Register는 1-Wire Host Core Controller가 실행 상태를 외부 프로세서에 보고하는 데 사용됩니다. 이 레지스터의 기능은 다음 표에 자세히 설명되어 있습니다.
| 비트 | 필드 이름 | 설명 |
|--|--|--|
| [31] | Presence | 0: 1-Wire Host Core Controller가 Reset/Presence 펄스 시퀀스 중 1-Wire 버스에서 장치를 감지했습니다. <br/> 1: 1-Wire Host Core Controller가 Reset/Presence 펄스 시퀀스 중 1-Wire 버스에서 장치를 감지하지 못했습니다.|
| [4] | Ready | 0: 1-Wire Host Core Controller가 다음 명령을 실행할 준비가 되지 않았습니다. 현재 명령을 실행 중이거나 *GO* 신호가 클리어되기를 기다리는 중입니다. <br/> 1 : 1-Wire Host Core Controller가 새 명령을 수신하고 실행할 준비가 되었습니다.|
| [0] | Done | 0: 1-Wire Host Core Controller가 명령 실행을 완료하고 수신 데이터(해당하는 경우)를 레지스터에 기록했습니다. <br/> 1: 1-Wire Host Core Controller가 명령 실행을 완료하지 않았습니다.|

### AXI 1-Wire Host 수신 데이터 레지스터(w1_RXDATA)

AXI 1-Wire Host Received Data Register는 1-Wire Host Core Controller가 수신 데이터를 저장하는 데 사용됩니다. 이 레지스터의 기능은 다음 표에 자세히 설명되어 있습니다.
| 비트 | 필드 이름 | 설명 |
|--|--|--|
| [7-0] | Read Data | 1-Wire 버스에서 수신한 비트입니다. 컨트롤러 명령이 *Read Bit*인 경우 비트가 LSB에 저장되고, 명령이 *Read Byte*인 경우 8비트가 모두 저장됩니다.|

### AXI 1-Wire Host 수신 데이터 레지스터(w1_GPIODATA)

AXI 1-Wire Host GPIO Read Data Register는 1-Wire 버스 레벨을 저장하는 데 사용됩니다. 이 레지스터의 기능은 다음 표에 자세히 설명되어 있습니다.
| 비트 | 필드 이름 | 설명 |
|--|--|--|
| [0] | Read GPIO | 1-Wire 버스 레벨이 지속적으로 저장됩니다. AXI 1-Wire Host를 GPIO 모드로 사용할 때 버스 레벨을 읽거나 1-Wire 버스 레벨을 모니터링하는 데 사용할 수 있습니다. |

## 프로그래밍 시퀀스

다음 단계는 AXI 1-Wire Host에 접근하는 데 도움이 됩니다.

### 1-Wire Host Core Controller 사용

 1. 코어를 리셋합니다(`w1_CTRL` 레지스터에 `0x8000_0000` 쓰기).
 2. Ready 인터럽트 마스크를 활성화합니다(w1_IRQCTRL 레지스터에 `0x0000_0010` 쓰기).
 3. 인터럽트가 발생할 때까지 기다린 다음 `w1_STAT` 레지스터를 읽어 Ready 신호가 인터럽트를 발생시켰는지 확인합니다.
 4. 인터럽트 마스크를 클리어합니다(w1_IRQCTRL 레지스터에 `0x0000_0000` 쓰기).
 5. 명령을 씁니다(`w1_INSTR`에 `0x0000_0*zyy*` 쓰기. *z*는 명령이고 *yy*는 해당하는 경우 전송할 데이터입니다. 예를 들어 바이트 0110_1010을 전송하려면 명령은 0x0000_0F6A입니다).
 6. Go 신호를 보내고 리셋을 클리어합니다(`w1_CTRL`에 `0x0000_0001` 쓰기).
 7. Done 인터럽트 마스크를 활성화합니다(w1_IRQCTRL 레지스터에 `0x0000_0001` 쓰기).
 8. 인터럽트가 발생할 때까지 기다리고 `w1_STAT` 레지스터를 읽어 Done 신호가 인터럽트를 발생시켰는지 확인합니다.
 9. 인터럽트 마스크를 클리어합니다(`w1_IRQCTRL` 레지스터에 `0x0000_0000` 쓰기).
 10. 해당하는 경우, 수신 비트를 가져오기 위해 데이터 레지스터(`w1_RXDATA`)를 읽습니다.
 11. Go 신호를 클리어합니다(`w1_CTRL`에 `0x0000_0000` 쓰기).
 12. 2단계로 돌아갑니다.

2, 3, 4, 7, 8, 9단계는 인터럽트 사용을 전제로 합니다. 인터럽트를 사용하지 않는 경우, 2, 3, 4단계는 `w1_STAT`에서 Ready 값이 1이 될 때까지 읽는 루프로 대체할 수 있으며, 7, 8, 9단계는 `w1_STAT`에서 Done 값이 1이 될 때까지 읽는 루프로 대체할 수 있습니다.

### GPIO 사용

1-Wire 버스 레벨 읽기:

 1. AXI 1-Wire Host를 GPIO 읽기로 구성합니다(w1_INSTR에 0x8080_0000 쓰기).
 2. `w1_GPIODATA` 레지스터를 읽습니다.

1-Wire 버스에 쓰기:

 1. AXI 1-Wire Host를 GPIO 쓰기로 구성합니다(`w1_INSTR`에 `0x808*y*_0000` 쓰기. *y*를 0으로 바꾸면 0을 쓰고, 1로 바꾸면 1을 씁니다).

## 드라이버

### Linux 드라이버

업스트림된 **AMD 프로그래머블 로직 IP 코어용 AXI 1-wire host driver** Linux 드라이버를 Linux 시스템에서 사용하여 AXI 1-Wire host와 인터페이스할 수 있습니다.

### 베어메탈 드라이버

AMD Vitis&trade;를 사용하여 애플리케이션을 개발할 수 있도록 AXI 1-Wire Host와 함께 기본적인 베어메탈 드라이버가 제공됩니다. 이 드라이버는 인터럽트를 구현하지 않습니다. 인터럽트를 사용하려면 자체 베어메탈 드라이버 버전을 구현하는 것이 권장됩니다. 제공된 베어메탈 드라이버를 포함하려면 *axi_1wire_host.h* 파일을 포함하십시오. 제공되는 드라이버 함수는 다음과 같습니다.

```
 /**
 *
 * 1-Wire Microcontroller를 리셋합니다.
 * 
 * @param baseaddr은 작업할 AXI_1WIRE_HOST 인스턴스의 기본 주소입니다.
 * 
 * @return
 * 
 */
 void AXI_1WIRE_HOST_Reset(u32 baseaddr);
 
 /**
 *
 * touch-bit 함수를 수행합니다. 0 또는 1을 쓰고 버스 레벨을 읽습니다.
 *
 * @param baseaddr은 작업할 AXI_1WIRE_HOST 인스턴스의 기본 주소입니다.
 *        bit는 쓸 레벨입니다.
 * 
 * @return 읽은 레벨
 * 
 */
 u8 AXI_1WIRE_HOST_TouchBit(u32 baseaddr, u8 bit);
 
 /**
 *
 * read-byte 함수를 수행합니다.
 * 
 * @param baseaddr은 작업할 AXI_1WIRE_HOST 인스턴스의 기본 주소입니다.
 * 
 * @return 읽은 값
 * 
 */
 u8 AXI_1WIRE_HOST_ReadByte(u32 baseaddr);
 /**
 *
 * write-byte 함수를 수행합니다.
 * 
 * @param baseaddr은 작업할 AXI_1WIRE_HOST 인스턴스의 기본 주소입니다.
 *        byte는 쓸 바이트입니다.
 * 
 */
 void AXI_1WIRE_HOST_WriteByte(u32 baseaddr, u8 byte);
 
 /**
 *
 * Reset-Presence 함수를 수행합니다.
 * 
 * @param baseaddr은 작업할 AXI_1WIRE_HOST 인스턴스의 기본 주소입니다.
 * 
 * @return 0=장치 있음, 1=장치 없음
 * 
 */
 u8 AXI_1WIRE_HOST_ResetBus(u32 baseaddr);
 
 /**
 *
 * 드라이버/장치에서 자체 테스트를 실행합니다. 장치 리셋이 수행되는 경우 이 테스트는 파괴적인 테스트일 수 있습니다.
 * 
 * 하드웨어 시스템이 올바르게 빌드되지 않은 경우, 이 함수가 호출자에게 반환되지 않을 수 있습니다.
 *
 * @param baseaddr은 작업할 AXI_1WIRE_HOST 인스턴스의 기본 주소입니다.
 *
 * @return
 *
 * - 모든 자체 테스트 코드가 통과하면 XST_SUCCESS
 * - 자체 테스트 코드 중 하나라도 실패하면 XST_FAILURE
 * 
 * @note 이 함수가 동작하려면 캐싱을 꺼야 합니다.
 * @note 데이터 메모리와 장치가 같은 버스에 있지 않으면 자체 테스트가 실패할 수 있습니다.
 *
 */
 XStatus AXI_1WIRE_HOST_SelfTest(u32 baseaddr);
 
 /**
 *
 * 1-Wire 버스 레벨을 읽습니다. 1-Wire 버스는 GPIO를 통해 제어됩니다.
 *
 * @param baseaddr은 작업할 AXI_1WIRE_HOST 인스턴스의 기본 주소입니다.
 *
 * @return 버스 레벨
 *
 */
 u8 AXI_1WIRE_HOST_GPIO_Read(u32 baseaddr);
 
 /**
 *
 * 1-Wire 버스 레벨을 설정합니다. 1-Wire 버스는 GPIO를 통해 제어됩니다.
 * 
 * @param baseaddr은 작업할 AXI_1WIRE_HOST 인스턴스의 기본 주소입니다.
 *        bit는 버스 레벨입니다.
 *
 * @return
 *
 */
 void AXI_1WIRE_HOST_GPIO_Write(u32 baseaddr, u8 bit);
```

## 예제 설계 흐름

이 섹션에서는 AMD Kria&trade; KD240 Drives Starter Kit를 대상으로 하는 설계를 생성하는 과정을 설명합니다. 여기에는 AXI 1-Wire Host의 사용자 지정 및 생성, 코어 제약, 하드웨어 설계를 생성하기 위한 합성 및 구현 단계, 소프트웨어 통합이 포함됩니다.

이 설계 흐름에는 AMD Vivado™ 및 Vitis Unified Software Platform 개발 도구가 필요합니다. 2023.1 및 2023.2 도구 릴리스에서 테스트되었습니다.

### AXI 1-Wire Host 사용자 지정 및 생성

이 섹션에는 Vivado Design Suite IP Integrator를 사용하여 블록 설계를 생성하고 AXI 1-Wire Host를 사용자 지정 및 생성하는 방법에 대한 정보가 포함되어 있습니다. IP Integrator를 사용하여 하드웨어 설계를 생성하는 방법에 대한 자세한 내용은 *Vivado Design Suite User Guide: Designing IP Subsystems Using IP Integrator* [(UG994)](https://docs.amd.com/access/sources/dita/map?isLatest=true&url=ug994-vivado-ip-subsystems&ft:locale=en-US)를 참조하십시오. 다음 단계는 Kria KD240 Drives Starter Kit를 위한 완전한 하드웨어 설계를 생성하는 과정을 설명합니다. 다른 장치를 대상으로 할 때도 일부 조정을 통해 동일한 과정을 사용할 수 있습니다.

1. 컴패니언 카드를 포함한 Kria KD240 Drives Starter Kit를 대상으로 새 Vivado 프로젝트를 생성합니다.
2. Block Design을 생성합니다.
3. 새 Block Design에서 AMD Zynq™ UltraScale+™ MPSoC IP를 추가하고 *Run Block Automation*을 실행하여 Board Preset을 적용합니다.
4. 블록 설계에 AXI 1-Wire Bus Host를 추가하고 기본 설정으로 *Run Connection Automation*을 실행합니다. 이렇게 하면 Processor System Reset 및 AXI Interconnect 블록이 추가됩니다. 또한 AMD Zynq UltraScale+ MPSoC `pl_clk0` 100MHz 클록에 클록을 연결하고, 모든 리셋 포트 및 AXI 인터페이스도 연결합니다.
5. 두 번째 AMD Zynq UltraScale+ MPSoC AXI 인터페이스를 연결하기 위해 기본 설정으로 *Run Connection Automation*을 한 번 더 실행합니다.
6. AXI 1-Wire Bus Host w1_irq 포트를 AMD Zynq UltraScale+ MPSoC pl_ps_irq0[0:0] 포트에 연결합니다.
7. AXI 1-Wire Bus Host w1_bus 포트를 마우스 오른쪽 버튼으로 클릭하고 **Make External**을 클릭합니다. 최종 블록 설계는 다음과 유사해야 합니다.
![블록 다이어그램](./images/block_design.png)
8. AXI 1-Wire Bus Host를 더블 클릭하면 사용자 지정 GUI가 표시됩니다. 볼 수 있듯이 S00 AXI CLK DIVIDER의 기본값은 100입니다. AXI 1-Wire Bus Host의 `s00_axi_aclk` 입력에 100MHz 클록을 사용하지 않는 경우, 이 값을 그에 맞게 조정해야 합니다.
![사용자 지정 GUI](./images/customization_gui.png)
9. 이제 블록 설계를 Validate할 수 있으며 오류가 없어야 합니다. Address Editor 탭을 보면 /axi_1wire_host_0/S00_AXI 인터페이스의 Base Address가 표시됩니다. 다른 AXI 블록이 설계에 있는 경우 다를 수 있지만, 대개 0xA000_0000일 것입니다.
10. *Sources*에서 *Hierarchy* 탭을 선택합니다. *Design Sources* 폴더를 펼치고 블록 다이어그램을 마우스 오른쪽 버튼으로 클릭한 다음 **Create HDL Wrapper**를 선택합니다. **Let Vivado manage wrapper and auto-update**를 선택하고 **OK**를 클릭합니다.
11. 새 constraints source 파일을 생성하고 다음 내용을 추가합니다. 이렇게 하면 w1_bus 포트가 KD240 1-Wire 포트에 연결되고 적절한 전압 및 풀업 저항이 설정됩니다.

	```
	set_property PACKAGE_PIN H13 [get_ports w1_bus_0]
	set_property IOSTANDARD LVCMOS33 [get_ports w1_bus_0]
	set_property PULLUP true [get_ports w1_bus_0]
	```

12. 이제 비트스트림을 생성하고 Hardware Platform(File&rarr;Export&rarr;Export Hardware)을 내보낼 수 있습니다. 비트스트림을 포함해야 하며, 이렇게 하면 XSA 파일이 생성됩니다.

### Standalone Software Platform 생성

이 섹션에는 Vitis Unified Software Platform을 사용하여 AXI 1-Wire Host를 대상으로 하는 애플리케이션을 실행할 플랫폼을 생성하는 방법에 대한 정보가 포함되어 있습니다. 플랫폼 및 애플리케이션 생성 방법에 대한 자세한 내용은 *Vitis Unified Software Platform Documentation: Application Acceleration Development* [(UG1393)](https://docs.amd.com/access/sources/dita/map?isLatest=true&url=ug1393-vitis-application-acceleration&ft:locale=en-US)를 참조하십시오.

1. 새 빈 Vitis Workspace를 생성합니다.
2. Application Project를 생성합니다.
3. 플랫폼의 경우, 이전에 Vivado로 생성한 XSA를 사용하여 하드웨어(XSA)로부터 새 플랫폼을 생성합니다.

   - *Generate boot componenets*를 체크한 상태로 유지합니다.
   - 첫 번째 단계 부트 로더(FSBL)를 생성하기 위한 Target processor로 *psu_cortexa53_0*을 선택합니다.
   - Application의 경우, 대상 프로세서로 *psu_cortexa53_0*을 선택합니다.
   - Domain의 경우, Operating System으로 standalone을 선택하고 Architecture로 64-bit를 선택합니다.
   - *Empty Application(C)* 템플릿을 선택합니다.
4. 워크스페이스는 다음 이미지와 유사해야 합니다. AXI 1-Wire Host standalone 드라이버는 *\<platform>/hw/drivers/axi_1wire_host_v1_0/src/* 아래에서 찾을 수 있습니다. 드라이버 파일은 C 헤더 파일(*axi_1wire_host.h*)과 두 개의 C 소스 파일(*axi_1wire_host.c* 및 *axi_1wire_host_selftest.c*)로 구성됩니다. 자세한 내용은 해당 파일을 열어 확인할 수 있습니다.
![Vitis 워크스페이스](./images/vitis_init.png)
5. 이제 제공된 드라이버 함수를 활용하여 AXI 1-Wire Host를 대상으로 하는 자체 애플리케이션을 생성할 수 있습니다.

   - AXI_1WIRE_HOST_Reset
   - AXI_1WIRE_HOST_TouchBit
   - AXI_1WIRE_HOST_ReadByte
   - AXI_1WIRE_HOST_WriteByte
   - AXI_1WIRE_HOST_ResetBus
   - AXI_1WIRE_HOST_SelfTest
   - AXI_1WIRE_HOST_GPIO_Read
   - AXI_1WIRE_HOST_GPIO_Write

<hr class="sphinxhide"></hr>

<p class="sphinxhide" align="center"><sub>Copyright © 2023–2024 Advanced Micro Devices, Inc.</sub></p>

<p class="sphinxhide" align="center"><sup><a href="https://www.amd.com/en/corporate/copyright">이용 약관</a></sup></p>
