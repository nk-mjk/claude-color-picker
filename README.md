![color-picker-from-sref](color-picker-from-sref_title.png)

# color-picker-from-sref

> 미드저니 sRef 그리드 이미지에서 키 컬러를 자동 추출하는 Claude Cowork 스킬

미드저니 sRef 코드로 생성한 3×3 스타일 샘플 이미지를 입력하면, **K-means + LLM 비전 하이브리드 파이프라인**으로 스타일을 대표하는 키 컬러셋을 추출합니다. 추출된 컬러셋은 이후 PPT·카드뉴스·문서 제작 시 "이 컬러로 만들어줘" 한 마디로 바로 활용할 수 있어요.

---

## 작동 원리

```
sRef 3×3 그리드 이미지
        ↓
[Phase 1] K-means 후보 추출
  Python 스크립트로 9개 셀 + 전체 이미지의 색상 후보를 계산
        ↓
[Phase 2] LLM 비전 키 컬러 선정
  Claude가 이미지를 직접 보면서 스타일 대표 색상 선별
  역할 라벨(Primary / Accent / Background 등) + 스타일 키워드 추출
        ↓
[Phase 3] 팔레트 시각화 + 메모리 저장
  인터랙티브 컬러 위젯 표시 · 이후 작업에서 즉시 참조 가능
        ↓
[Phase 4] colorset_{sref_code}.md 파일 저장 (선택)
```

---

## 사용 방법

### 준비물

미드저니 sRef 3×3 그리드 이미지 (PNG)

> **그리드 이미지가 없다면?**  
> 스킬 실행 시 `sref-grid.html` 뷰어를 제공받을 수 있어요.  
> sRef 코드 입력 → Load → "Download All as One"으로 PNG 저장 → 작업 폴더에 복사

### 실행 키워드

```
sref 컬러 뽑아줘
이 이미지에서 색 추출해줘
컬러피커 시작
color-picker 시작
sref 색상 추출해줘
```

### 실행 흐름

1. 스킬 시작 → 작업 폴더의 PNG 파일명 전달
2. K-means 자동 분석 (수 초 소요)
3. Claude 비전으로 키 컬러 선정 + 팔레트 위젯 표시
4. `colorset_{sref_code}.md` 저장 여부 선택

---

## 출력 예시

### 팔레트 위젯

스킬 실행 후 대화창에 인터랙티브 팔레트 위젯이 표시됩니다.  
각 색상 카드에는 HEX 코드 · 역할 · 설명이 함께 표시됩니다.

### colorset 파일

```markdown
# sRef 1025028912 컬러셋

- 스타일 키워드: ukiyo-e, woodblock, flat, indigo, earth-tones
- 무드: 일본 목판화 특유의 인디고 남색 + 화지 베이지 + 번트오렌지 포인트

| HEX       | 역할        | 설명                     |
|-----------|-------------|--------------------------|
| `#D7BB9F` | Background  | 화지(和紙) 베이지 · 배경  |
| `#EC4F13` | Accent      | 번트오렌지 · 핵심 포인트  |
| `#0B5C7A` | Primary     | 인디고 블루 · 남색 주조   |
| `#0D3250` | Deep Indigo | 딥 네이비 · 그림자/심도   |
| `#0E8D96` | Teal        | 청록 · 물/하늘 보조 포인트|
| `#939B92` | Neutral     | 세이지그레이 · 중간톤     |
| `#36363A` | Outline     | 차콜 · 윤곽선/아웃라인    |
```

### 이후 활용

colorset 파일을 새 대화에 첨부하면 해당 컬러셋을 다른 스킬에서 즉시 사용할 수 있어요.

```
[colorset_1025028912.md 첨부]
"이 컬러로 PPT 만들어줘"
"이 스타일로 카드뉴스 제작해줘"
```

---

## 포함 파일

| 파일 | 설명 |
|------|------|
| `SKILL.md` | 스킬 실행 지침 |
| `scripts/sref_candidates.py` | K-means 후보 추출 스크립트 |
| `tools/sref-grid.html` | sRef 3×3 그리드 뷰어 |

---

## 연계 스킬

| 스킬 | 용도 |
|------|------|
| `design-system-by-blue` | colorset → bds_ 디자인 시스템 파일로 확장 |
| `pptx` / `docx` | 추출한 컬러셋으로 슬라이드 · 문서 제작 |
| `mj-prompt-guide-by-blue` | 미드저니 프롬프트 작성 |
| `image-cardnews` / `ck-cardnews` | 컬러셋 적용 카드뉴스 제작 |

---

*Made by Blue (조남경) · [미드저니 코리아](https://www.facebook.com/groups/midjourneykorea)*
