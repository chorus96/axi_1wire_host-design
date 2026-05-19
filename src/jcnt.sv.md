# jcnt.v 분석 (암호화됨)

## 상태
- `src/jcnt.v` 파일은 `pragma protect`로 암호화되어 내부 RTL을 확인할 수 없습니다.
- 내부 카운터 구현(Johnson counter 방식 등)은 `w1_master.v` 내 인스턴스 사용 맥락으로만 추정 가능합니다.

## Block Diagram
```mermaid
flowchart LR
  CLK[clk] --> JCNT[jcnt.v (Encrypted)] --> Q[q[W-1:0]]
  RST[reset] --> JCNT
  EN[en] --> JCNT
```

## 비고
- `w1_master.v`에서 10-bit, 2-bit 두 종류로 파라미터화 인스턴스되어 타임슬롯 생성을 담당합니다.
