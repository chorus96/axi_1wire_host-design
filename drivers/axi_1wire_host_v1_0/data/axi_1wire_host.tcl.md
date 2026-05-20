# `axi_1wire_host.tcl` Analysis

## 역할 요약
이 드라이버 TCL은 Vitis/BSP 생성 시 `axi_1wire_host` IP의 인스턴스 정보를 `xparameters.h`에 내보내는 **메타데이터 생성 스크립트**입니다.

핵심은 `generate {drv_handle}` 프로시저 하나이며, `xdefine_include_file` 호출을 통해 다음 매크로 항목을 생성 대상으로 지정합니다.

- `NUM_INSTANCES`
- `DEVICE_ID`
- `C_S00_AXI_BASEADDR`
- `C_S00_AXI_HIGHADDR`

## 동작 흐름
1. BSP/드라이버 생성 과정에서 `generate`가 호출됩니다.
2. 핸들(`drv_handle`)을 기준으로 드라이버 정보가 조회됩니다.
3. `xparameters.h`에 `axi_1wire_host` 관련 매크로가 기록됩니다.

## Block Diagram
```mermaid
flowchart TD
    A[BSP Driver Generation] --> B[generate(drv_handle)]
    B --> C[xdefine_include_file 호출]
    C --> D[xparameters.h]
    D --> E[NUM_INSTANCES]
    D --> F[DEVICE_ID]
    D --> G[C_S00_AXI_BASEADDR]
    D --> H[C_S00_AXI_HIGHADDR]
```

## 주석
스크립트가 매우 간결하여, 동작의 대부분은 Xilinx 도구체인의 `xdefine_include_file` 표준 동작에 의해 결정됩니다.
