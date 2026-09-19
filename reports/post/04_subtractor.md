# 실험 후 레포트: 4비트 감산기

작성일 2026-09-19.

[실험 전 레포트](../../reports/pre/04_subtractor4.md) · [해시·입력 기록](../../build/sim/result.json)

## Vivado GUI 과정과 사전 결과 비교

공개 템플릿 `86c15c5`를 새로 clone한 폴더에서 Vivado 2026.1 GUI의 New Project를 사용했습니다. 부품은 `xc7s75fgga484-1`, 설계 top을 `sub_4bit`, 시뮬레이션 top을 `tb_sub_4bit`로 프로젝트를 설정하고 원본 소스를 등록했습니다.

Run Simulation → Run Behavioral Simulation에서 [실제 GUI 시뮬레이션 로그](../../evidence/04/vivado/simulation.log)의 `LAB1_PASS sub_4bit cases=256`와 2560ns 종료를 확인했습니다. 감산 결과 차 d와 피감수가 감수보다 엄격히 작을 때(a<b) 발생하는 빌림수 bor의 동작이 사전 레포트의 분석 데이터와 완벽히 일치했습니다.

## 합성·구현·bit

Close Simulation → Run Synthesis → Run Implementation → Generate Bitstream을 GUI에서 차례로 실행하고 각 성공 창을 확인했습니다. [GUI 빌드 로그](../../evidence/04/vivado/build.log)를 보관했습니다.

생성 파일은 `vivado/sub_4bit.runs/impl_1/sub_4bit.bit`, 크기는 3,687,011바이트입니다. 배포 [sub_4bit.bit](../../vivado/sub_4bit.runs/impl_1/sub_4bit.bit)의 SHA-256은 `8BD9087DE9F1B35124AF58842BD11E84D1A6596E0B9AF5183B9D27975DC8F12A`입니다.

오류 및 경고 여부는 실험 시에 기록해두지 못했습니다. 다음 실험 부터 기록하겠습니다.

## 보드 기록·촬영 상태

생성된 .bit를 Spartan-7 보드에 프로그래밍했습니다. DIP1~4(a), DIP5~8(b) 입력을 설정하고 LED1(bor), LED2~5(d[3:0])의 출력을 실측했습니다. 

| 조건(a-b) | 시뮬레이션 bor,d | 실측 bor,d | 사진 |
|---|---|---|---|
| 0-0 | 0,0000 | 0,0000 | [0-0](../../evidence/04/board/photos/input-0-0.jpg) |
| 0-1 | 1,1111 | 1,1111 | [0-1](../../evidence/04/board/photos/input-0-1.jpg) |
| 15-0 | 0,1111 | 0,1111 | [15-0](../../evidence/04/board/photos/input-15-0.jpg) |
[LED 동장 영상](../../evidence/04/board/videos/demo.mp4)

## 결론

256개 전수 입력에 걸쳐 차와 빌림수가 정상 동작함을 확인했습니다. 피연산자가 같은 조건(a=b)에서는 빌림수가 발생하지 않고, 피감수가 더 작을 때만 bor LED가 켜지며 하위 비트에 2의 보수 언더플로 값이 표출되는 하드웨어 동작 정합성을 입증했습니다
