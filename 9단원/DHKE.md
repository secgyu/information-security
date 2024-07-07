# 📘 Diffie-Hellman Key Exchange (DHKE)

이 레포지토리는 Diffie-Hellman Key Exchange (DHKE) 알고리즘과 관련된 내용을 설명하고 구현한 것입니다. 각 주제에 대한 간략한 설명과 관련 알고리즘을 소개합니다.

### 🔒 DHKE 개요
Diffie-Hellman Key Exchange는 1976년에 소개된 최초의 공개키 알고리즘으로, 두 사용자 간에 안전하지 않은 채널을 통해 안전하게 대칭키를 공유할 수 있는 방법을 제공합니다. 이 알고리즘은 이산 로그 문제의 어려움에 기반을 두고 있습니다.

### 📝 DHKE 예
- **Alice**: 
  - 큰 소수 p와 Zp*에서의 원시근 α는 공개되어 있다고 가정.
  - Alice는 개인값 a를 선택 (1 ≤ a ≤ p - 1).
  - Alice는 A = α^a mod p를 계산하여 얻은 공개값 A를 Bob에게 송신.
  - Alice는 Bob의 공개값 B를 평문으로 수신.
  - Alice는 K = B^a mod p를 계산하여 대칭키 K를 획득.

- **Bob**: 
  - 큰 소수 p와 Zp*에서의 원시근 α는 공개되어 있다고 가정.
  - Bob은 개인값 b를 선택 (1 ≤ b ≤ p - 1).
  - Bob은 B = α^b mod p를 계산하여 얻은 공개값 B를 Alice에게 송신.
  - Bob은 Alice의 공개값 A를 평문으로 수신.
  - Bob은 K = A^b mod p를 계산하여 대칭키 K를 획득.

### 👾 Man-in-the-Middle 공격
Man-in-the-Middle 공격은 공격자가 Alice와 Bob 사이의 통신을 가로채고, 각자와 별도로 키 교환을 수행하여 각각 다른 대칭키를 확보하는 공격 방식입니다.

---

### 구현 예제

#### 기본 DHKE 알고리즘
```python
import random

def generate_private_key(p):
    return random.randint(1, p - 1)

def compute_public_key(alpha, private_key, p):
    return pow(alpha, private_key, p)

def compute_shared_secret(public_key, private_key, p):
    return pow(public_key, private_key, p)

# 예제 사용법
p = 23  # 큰 소수
alpha = 5  # 원시근

# Alice
alice_private = generate_private_key(p)
alice_public = compute_public_key(alpha, alice_private, p)

# Bob
bob_private = generate_private_key(p)
bob_public = compute_public_key(alpha, bob_private, p)

# 공유 대칭키 계산
alice_shared_secret = compute_shared_secret(bob_public, alice_private, p)
bob_shared_secret = compute_shared_secret(alice_public, bob_private, p)

print(f"Alice의 공유 대칭키: {alice_shared_secret}")
print(f"Bob의 공유 대칭키: {bob_shared_secret}")
