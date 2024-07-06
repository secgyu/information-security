# 📘 RSA 알고리즘

## 📑 목차
1. RSA 개요
2. RSA 예
3. 보안 강도
4. RSA 알고리즘
   - 키 생성
   - 암호화
   - 복호화
5. RSA 공격자, 인수분해 문제
6. RSA 소수 선택
7. Square-and-Multiply Algorithm
8. RSA 복호화에서 평문이 복원되는 이유
9. 참고 문헌

## 🔍 RSA 개요
RSA(Rivest-Shamir-Adleman) 알고리즘은 공개키 암호 기술의 하나로, 1977년 Ron Rivest, Adi Shamir, Len Adleman에 의해 제안되었습니다. RSA는 큰 소수의 곱을 사용하여 키를 생성하며, 공개키와 개인키 쌍을 통해 데이터를 암호화하고 복호화할 수 있습니다.

## 📊 RSA 예
### Bob의 키 생성 예시
- 두 소수 p = 7, q = 11 선택
- n = p * q = 7 * 11 = 77 계산
- φ(n) = (p - 1) * (q - 1) = 60 계산
- gcd(φ(n), e) = 1, 1 < e < φ(n) 조건을 만족하는 e = 13 선택
- e * d ≡ 1 (mod φ(n))을 만족하는 d = 37 계산
- 공개키 e, n = (13, 77) 공개
- 개인키 d, n = (37, 77) 안전하게 보관

### 암호화와 복호화 예시
- Alice가 보낸 암호문 C = 26 수신
- 개인키 d, n = (37, 77)로 암호문 복호화
  - T = C^d mod n = 26^37 mod 77 = 5
- Alice가 보낸 평문 5 수신
- Alice가 Bob에게 평문 T = 5를 전송하려 함
  - 공개키 e, n = (13, 77)로 평문 암호화
  - C = T^e mod n = 5^13 mod 77 = 26
- Bob에게 암호문 C = 26 전송

## 🔐 보안 강도
RSA 알고리즘의 보안 강도는 사용되는 소수의 크기와 키 길이에 크게 의존합니다. NIST의 권고에 따르면, RSA 키 길이는 최소 2048비트 이상이어야 하며, 2030년 이후에는 3072비트 이상의 키를 사용하는 것이 좋습니다.

## ⚙️ RSA 알고리즘
### 🔑 키 생성
1. 두 소수 p와 q 선택 (단, p ≠ q)
2. n = p * q 계산
3. φ(n) = (p - 1) * (q - 1) 계산
4. gcd(φ(n), e) = 1을 만족하는 e 선택 (1 < e < φ(n))
5. e * d ≡ 1 (mod φ(n))을 만족하는 d 계산
6. 공개키 (e, n)와 개인키 (d, n) 생성

### 🔒 암호화
- 평문 T에 대해 (T < n), 암호문 C 생성
  - C = T^e mod n

### 🔓 복호화
- 암호문 C에 대해, 평문 T 복원
  - T = C^d mod n

## 🛡️ RSA 공격자, 인수분해 문제
공격자가 공개키 (e, n)을 알고 있다면, n을 소인수분해하여 p와 q를 구할 수 있으면 개인키 d를 쉽게 계산할 수 있습니다. RSA의 보안은 n의 소인수분해가 어렵다는 사실에 기반합니다.

## 🔍 RSA 소수 선택
### 소수 선택 절차
1. 임의의 홀수 n 랜덤 선택
2. 임의 정수 a < n 랜덤 선택
3. a를 인자로 하여 확률적 소수 테스트(예: Miller-Rabin) 수행. 실패 시 1단계로 이동
4. 테스트 성공 횟수가 충분하지 않으면 2단계로 이동
5. n을 소수로 채택

## 🔢 Square-and-Multiply Algorithm
모듈러 지수승 계산은 RSA에서 중요한 역할을 합니다. 이 알고리즘은 효율적으로 모듈러 지수승을 계산하는 방법을 제공합니다.

### 알고리즘
1. 비트를 사용하여 지수를 표현
2. v = 1로 초기화
3. 각 비트에 대해:
   - v = (v * v) % n
   - 비트가 1이면 v = (v * a) % n
4. 최종 v 반환

### 예제
```python
def square_and_multiply(a, k, n):
    bits = '{:b}'.format(k)
    v = 1
    for b in bits:
        v = (v * v) % n
        if b == '1':
            v = (v * a) % n
    return v

c, d, n = 26, 37, 77
print(square_and_multiply(c, d, n))  # Output: 5
```

## 🔎 RSA 복호화에서 평문이 복원되는 이유
1. e * d ≡ 1 (mod φ(n))에서 e * d = 1 + k * φ(n)을 만족
2. C^d mod n = (T^e)^d mod n = T^(e * d) mod n = T

## 📚 참고 문헌
- [NIST Special Publication 800-57pt1r5](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r5.pdf)
