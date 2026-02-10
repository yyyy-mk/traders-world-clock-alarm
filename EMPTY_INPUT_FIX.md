# 🔧 빈 입력값 자동 0 처리 기능 추가

## 📝 수정 내용

### 수정된 함수들

#### 1. **addAlarm()** - 알람 추가 함수
```javascript
// 수정 전
hour: parseInt(document.getElementById('alarmHour').value)
minute: parseInt(document.getElementById('alarmMinute').value)

// 수정 후
const hour = parseInt(document.getElementById('alarmHour').value) || 0;
const minute = parseInt(document.getElementById('alarmMinute').value) || 0;
```

**효과:**
- 시간 입력란이 비어있으면 → 자동으로 0시
- 분 입력란이 비어있으면 → 자동으로 0분

#### 2. **addTimer()** - 타이머 추가 함수
```javascript
// 수정 전
const m = parseInt(document.getElementById('timerMin').value);
const s = parseInt(document.getElementById('timerSec').value);

// 수정 후
const m = parseInt(document.getElementById('timerMin').value) || 0;
const s = parseInt(document.getElementById('timerSec').value) || 0;
if (m === 0 && s === 0) { alert('타이머 시간을 0보다 크게 설정하세요!'); return; }
```

**효과:**
- 분 입력란이 비어있으면 → 자동으로 0분
- 초 입력란이 비어있으면 → 자동으로 0초
- 둘 다 0이면 → 에러 메시지 표시

#### 3. **updateKoreaPreview()** - 미리보기 함수
```javascript
// 수정 전
const h = document.getElementById('alarmHour').value;
const m = document.getElementById('alarmMinute').value;

// 수정 후
const h = parseInt(document.getElementById('alarmHour').value) || 0;
const m = parseInt(document.getElementById('alarmMinute').value) || 0;
```

**효과:**
- 실시간 미리보기에서도 빈 값을 0으로 처리
- 입력 중에도 깨지지 않음

## 🎯 사용 예시

### 알람 설정 예시

**예시 1: 시간만 입력**
```
시간: 9
분: (비어있음)
→ 결과: 09:00 알람 설정
```

**예시 2: 분만 입력**
```
시간: (비어있음)
분: 30
→ 결과: 00:30 알람 설정
```

**예시 3: 둘 다 비어있음**
```
시간: (비어있음)
분: (비어있음)
→ 결과: 00:00 알람 설정
```

### 타이머 설정 예시

**예시 1: 분만 입력**
```
분: 5
초: (비어있음)
→ 결과: 5분 0초 타이머 시작
```

**예시 2: 초만 입력**
```
분: (비어있음)
초: 30
→ 결과: 0분 30초 타이머 시작
```

**예시 3: 둘 다 비어있음**
```
분: (비어있음)
초: (비어있음)
→ 결과: "타이머 시간을 0보다 크게 설정하세요!" 경고
```

## 💡 기술 설명

### JavaScript의 `||` 연산자 활용

```javascript
parseInt("") → NaN (Not a Number)
parseInt("") || 0 → 0

parseInt("5") → 5
parseInt("5") || 0 → 5
```

**작동 원리:**
1. `parseInt("")`는 `NaN` 반환
2. `NaN || 0` → `0` (NaN은 falsy 값)
3. `parseInt("5")`는 `5` 반환
4. `5 || 0` → `5` (5는 truthy 값)

### Falsy 값 목록
JavaScript에서 `||` 연산자는 다음 값들을 false로 처리:
- `false`
- `0`
- `""` (빈 문자열)
- `null`
- `undefined`
- `NaN`

## ⚠️ 주의사항

### 장점
✅ 사용자가 빈 칸을 남겨도 에러 없이 작동
✅ 빠르게 입력 가능 (0을 일일이 입력할 필요 없음)
✅ 직관적인 동작

### 단점 (있을 수 있는 경우)
⚠️ 사용자가 실수로 빈 칸을 남긴 경우를 구분 못함
- 예: "9시 30분"을 입력하려다 분을 깜빡한 경우
- 9:00으로 설정됨 (9:30이 아님)

**해결책:**
현재는 자동으로 0으로 처리하는 것이 더 편리하다고 판단했습니다.
만약 명시적인 확인이 필요하다면:

```javascript
// 대안 코드 (사용하려면 교체)
const hour = parseInt(document.getElementById('alarmHour').value);
const minute = parseInt(document.getElementById('alarmMinute').value);

if (isNaN(hour) || isNaN(minute)) {
    alert('시간과 분을 모두 입력하세요!');
    return;
}
```

## 📊 테스트 체크리스트

### 알람 설정 테스트
- [x] 시간만 입력 → 00분으로 설정
- [x] 분만 입력 → 00시로 설정
- [x] 둘 다 비움 → 00:00으로 설정
- [x] 정상 입력 → 정상 작동
- [x] 미리보기 정상 작동

### 타이머 설정 테스트
- [x] 분만 입력 → 0초로 설정
- [x] 초만 입력 → 0분으로 설정
- [x] 둘 다 비움 → 에러 메시지
- [x] 정상 입력 → 정상 작동

## 🎉 결론

**모든 입력 시나리오에서 안정적으로 작동합니다!**

사용자가 입력란을 비워두어도:
- 알람: 자동으로 0으로 처리되어 설정됨
- 타이머: 0이면 에러 메시지, 한쪽만 입력해도 작동
- 미리보기: 실시간으로 정확하게 표시

더 편리하게 사용하실 수 있습니다! ✨
