# decimal

## 호출
```python
from decimal import Decimal, getcontext, localcontext, Context
from decimal import ROUND_HALF_EVEN, ROUND_HALF_UP, ROUND_DOWN, ROUND_UP, ROUND_CEILING, ROUND_05UP
```

## `Decimal` 메서드
- `quantize(exp, rounding=...)`
    - 소수 자릿수 고정
    - `d.quantize(Decimal('0.01'), rounding=ROUND_HALF_UP)`
- `normalize()`
    - trailing zero 제거
    - `Decimal('2.5000').normalize()` -> `Decimal('2.5')
- `as_tuple()`
    - 내부 표현(sign, digits, exponent) 반환
    - 내부 디버그/검증에 유용
- `as_integer_ratio()`
    - 정확한 정수 분수 표현
    - `(num, dem)` 반환
    - `Decimal('1.25').as_integer_ratio` -> `(5, 4)`
- `to_integral_value(rounding=None, context=None)`/`to_integral_exact(...)`
    - 정수로 반올림
    - `to_integral_exact`는 반올림 시 `Inexact`/`Rounded` 플래그를 올림
- `sqrt(context=None)`, `ln()`, `log10`, `exp()`
    - 고정밀 연산(context에 따라 반올림)
    - `Decimal(2).sqrt()`
- `copy_sign(other)`, `copy_abs()`, `copy_negate()`
    - 부호 조작
    - `d.copy_sign(Decimal('-1'))`
- `fma(other, third)`
    - `(self * other) + third`를 중간 반올림 없이 계산
- 비교 관련
    - `compare()`, `compare_signal()`, `compare_total()`, `compare_total_mag()`
    - NaN 처리, 총정렬 등 상황별 비교를 제공
    - `compare_total()`은 표현까지 포함한 전부를 비교
- 상태 확인
    - `is_nan()`, `is_qnan()`, `is_snan()`, `is_infinite()`, `is_finite()`, `is_zero()`, `is_subnormal()` 등
- `adjusted()`
    - 최상위 유효숫자 위치 얻기
- `to_eng_string()`
    - 공학적 표기 문자열 출력

## Context
연산의 **정밀도(prec), 반올림 모드(rounding), 지수 범위(Emin/Emax), flags/traps** 등을 결정

전역 기본 `context`는 `getcontext()`로 얻음

- `getcontext()`/`setcontext(ctx)`
- `localcontext(ctx=None)`
    - with 문으로 임시 변경
    ```python
    with localcontext() as ctx:
        ctx.prec=50
    ```
- `Context(prec=28, rounding=ROUND_HALF_EVEN, Emin=..., Emax=...)`
    - 사용자 컨택스트 생성
- `Context`의 주요 속성
    - `prec`
        유효 자릿수
    - `rounding`
        반올림 모드
    - `Emin`, `Emax`
        - 지수 범위
    - `flags`
        - 최근 발생한 신호
    - `traps`
        - 특정 신호를 예외로 만들기
        - `traps[Inexact] = True`
        - 플래그/트랩을 잘 사용해 규칙을 강제하거나, 특정 연산에서 예외를 발생시켜 제외 가능

## 반올림 모드
- `ROUND_HAFL_EVEN`
    - 은행 반올림(짝수로 라운드) = 기본값
- `ROUND_HALF_UP`
    - 0.5면 올림
- `ROUND_HALF_DOWN`
- `ROUND_DOWN`
    - 항상 버림(절대값 줄이기)
- `ROUND_UP`
    - 항상 올림(절대값 키우기)
- `ROUND_FLOOR`
    - 음수에 대해 더 작은 쪽으로
- `ROUND_CEILING`
    - 양수에 대해 더 큰 쪽으로
- `ROUND_05UP`
    - 마지막 자릿수가 0 또는 5이면 away-from-zero, 아니면 toward-zero

## Signals/Exceptions
- Signals (상태 종류)
    - flags/traps로 제어됨
    - 트랩이 커지면 관련 예외 발생
- 예외 클래스
    - `DecimalException`
    - `InvalidOperation`, `DivisionZero`, `Overflow`, `Underflow`, `Inexact`, `Rounded`, `Subnormal`, `Clamped`, `FloatOperation`, `DivisionUndefinded`, `DivisionImpossible` 등
- 계산 중 무조건 예외로 잡고 싶다면, `getcontext()traps[Inexact] = True`처럼 트랩을 킬 것

## 사용 예시
1. 전역 정밀도 설정
    ```python
    from decimal import getcontext
    getcontext().prec = 50
    ```

2. 지역 정밀도 설정
    ```python
    from decimal import localcontext, ROUND_HALF_UP
    with localcontext() as ctx:
        ctx.prec = 40
        ctx.rounding = ROUND_HALF_UP
        # 연산
    ```

3. 정확한 화폐 연산(둘째 자리까지 버림/반올림)
    ```python
    from decimal import Decimal, ROUND_HALF_UP
    TWOPLACES = Decimal('0.01')
    price = Decimal('19.995')
    total = price.quantize(TWOPLACES, rounding=ROUND_HALF_UP)
    ```

4. float 혼합 방지(실수 입력 방지)
    ```python
    from decimal import Decimal, getcontext
    getcontext().traps[FloatOperation] = True
    # float로 생성/비교 시 예외 발생
    # 대신 Decimal(str(f)) 또는 Decimal.from_float(...)를 신중히 사용
    ```

5. 정수-정밀도 기반 루틴
    - 큰 정밀도/큰 정수 작업 시
        - `getcontext().prec`를 충분히 키우고, `Emin/Emax` 조정 필요(메모리 주의)

## 성능, 주의사항
- `Decimal`은 정확성 우선, `float`보다 느림
- `Decimal`과 `float`연산은 섞지 말것
    - 섞어야 한다면 변환을 명확히 할 것
    - `Decimal(str(f))` 또는 `from_float`
- `quantize()`는 컨텍스트 정밀도에 의해 `InvalidOperation`이 발생할 수 있으므로 `prec`과 exp를 신경 쓸 것
