---
name: "receipt-to-pdf"
description: "영수증 사진들을 자동 크롭/원근보정/흑백처리한 후 PPT 슬라이드 크기 PDF·PPTX로 정리한다. 지출결의서 영수증 정리 작업에 사용."
version: "1.0.0"
---

# Receipt → PDF

영수증 사진(JPG)을 자동으로 처리해 정렬된 PDF/PPTX로 만든다.

## 처리 단계

1. **종이 마스크**: 밝기/채도 기반 + Otsu 자동 임계값으로 흰 종이 영역 추출
2. **가장 큰 블롭 분리**: 4-연결 컴포넌트 중 가장 큰 것만 남김 (다른 종이/반사 제거)
3. **bbox 추정**: 행/열 프로파일로 영수증 영역의 거친 경계 산출
4. **4코너 감지**: bbox 안에서 4개 엣지(상/하/좌/우)에 로버스트 직선 피팅 → 교점으로 코너 도출
5. **원근 변환**: 직사각형으로 펼치기
6. **흑백 + 대비 강화**: autocontrast → contrast 1.4배
7. **자동 회전**: 가로(W>H)면 90° 세로 정렬 (헤더 진한 쪽이 위로)
8. **레이아웃**: PPT 16:9 슬라이드(13.333"×7.5")에 5개씩 좌측·상단 정렬
9. **출력**: PDF + PPTX 동시 저장

## 회전이 거꾸로 됐을 때

자동 회전은 90° 정렬만 안정적으로 처리하고, 180° 뒤집힘 감지는 OCR 없이는 신뢰성 낮음.
결과 이미지가 거꾸로 나오면 `--flip` 옵션으로 파일명을 지정해 다시 돌린다:

```bash
python process.py receipt --flip 094610,094719
```

`--flip` 값은 파일 stem에 부분 매칭됨 (`20260429_094610.jpg` → `094610`만 써도 매칭).

## 동작 방식

`/receipt-to-pdf` 호출 시:
- 인자 없으면 → 현재 작업 디렉토리의 `receipt/` 폴더 처리, `receipt_out/`에 개별 이미지 저장, 작업 디렉토리에 `<폴더명>_영수증.pdf|pptx` 생성
- 인자 있으면 → `/receipt-to-pdf <input_dir> [output_name]`

처리할 이미지가 안 보이면 사용자에게 폴더 위치 확인 후 진행한다.

## 실행

`process.py`를 그대로 실행:

```bash
python "C:/Users/owen/.claude/skills/receipt-to-pdf/process.py" <input_dir> [output_name]
```

## 코너 감지 실패 시

자동 감지가 만족스럽지 않으면 사용자에게 안내:
- 원본 사진 4 귀퉁이에 빨간 점(8pt 이상) 마킹 후 별도 폴더에 두면 정확한 좌표로 처리 가능
- 빨간 점 모드: `process.py --points <points_dir>` 옵션 활용

## 의존성

- numpy, Pillow (필수)
- python-pptx (PPTX 출력용 — 없으면 PDF만 생성)
