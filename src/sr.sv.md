# sr.v 분석 (암호화됨)

## 상태
- `src/sr.v` 파일은 `pragma protect`로 암호화되어 내부 RTL을 확인할 수 없습니다.
- 따라서 포트/로직/FSM 상세 분석은 수행할 수 없습니다.

## Block Diagram
```mermaid
flowchart LR
  A[sr.v (Encrypted IP)] --> B[Unknown Internal Logic]
```

## 비고
- 복호화 가능한 원본 RTL 또는 시뮬레이션용 netlist가 있으면 상세 분석 문서를 갱신할 수 있습니다.
