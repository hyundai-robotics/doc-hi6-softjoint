
[__SOURCE](README.md)
# ${cont_model} 제어기 기능설명서 - softjoint 기능


[__SOURCE](0-about-this-manual/README.md)
# 이 설명서에 대하여


[__SOURCE](0-about-this-manual/precautions.md)
# 사전 주의사항

{% include file="ko/precautions.md" %}

[__SOURCE](0-about-this-manual/safety-notice.md)
# 안전 주의 사항

{% include file="ko/safety-notice.md" %}

{% hint style="warning" %}
- 외부 통신 명령 및 응용프로그램을 통한 제어는 안전 기능이 아니며, 안전 관련 제어 시스템을 대체할 수 없습니다.
- SafeSpace, 소프트조인트 등 안전 기능은 위험을 보조적으로 감소시키는 수단이며, 외부 안전펜스, 인터록 및 위험성 평가를 대체하지 않습니다.
{% endhint %}

[__SOURCE](1-intro/README.md)
# 1. 개요

**softjoint 기능**은 사용자가 설정한 환경을 기준으로, 로봇이 외력에 대해 **축 좌표계** 기준으로 유연하게 반응하도록 하는 기능입니다.  
기능을 정확하게 사용하기 위해서는 로봇에 장착된 **툴(Tool)** 또는 **부가 중량(Payload)** 정보를 정확히 설정해야 합니다.  

본 기능은 **소프트웨어 기반**으로 동작하므로, **힘/토크 센서 등 별도의 추가 하드웨어 없이** 사용할 수 있습니다.


---

[__SOURCE](2-main/README.md)
# 2. 명령어

SoftJoint 기능은 두 개의 명령어(`softjoint_lim`, `softjoint`)를 통해 설정 및 제어됩니다.  

- `softjoint_lim` 명령어는 SoftJoint 동작에 필요한 **기본 파라미터를 사전에 정의**하는 역할을 합니다.
- `softjoint` 명령어는, `softjoint_lim` 설정된 파라미터를 기준으로 **SoftJoint 기능을 활성화 또는 비활성화**합니다.

따라서 SoftJoint 기능을 사용하기 위해서는, 반드시 `softjoint_lim` 명령어를 먼저 사용하여 적용할 축과 동작 특성을 설정한 후 `softjoint on` 명령어를 통해 기능을 활성화해야 합니다.

`softjoint_lim` 명령어를 통해 사용자는 다음과 같은 항목을 정의할 수 있습니다.
- softjoint가 적용될 축 번호
- 외력에 대한 반응의 부드러움 정도
- 축의 허용 각도 범위
- 외력 감지를 위한 문턱값
[__SOURCE](2-main/2.1-softjoint.md)
## 2.1 softjoint

`softjoint` 기능은 별도의 힘/토크 센서를 사용하지 않고,  
외력이 인가될 경우 로봇이 **관절 축 좌표(Joint coordinate) 기준으로 순응(compliance) 동작을 수행하도록 제어하는 기능**입니다.

---

### 문법

```plaintext
softjoint on
softjoint off
```

---

### 파라미터

| 항목 | 설명 |
|-----|------|
| on  | softjoint 기능 시작 |
| off | softjoint 기능 종료 |

---
<br>

{% hint style="info" %}
`softjoint on` 기능을 사용하기 전에 반드시 `softjoint_lim`에서 다음 항목을 사전에 설정해야 합니다.

* 유연하게 동작할 축 번호 (`j`)
* 유연함 정도 (`sft`)
* 제한 각도 (`ang`)
* 문턱값 (`thr`)

외력에 대한 로봇의 민감도를 향상시키기 위해, `softjoint on` 명령어 실행 전에 `delay` 명령어를 사용하여 로봇을 약 **1~2초간 정지**시키는 것을 권장합니다.
{% endhint %}
<br>

{% hint style="warning" %}
부가축은 해당 기능을 사용할 수 없습니다. 
{% endhint %}


[__SOURCE](2-main/2.2-softjoint_lim.md)
## 2.2 softjoint_lim 

`softjoint_lim` 명령어는 **softjoint on** 기능을 사용하기 전에  
SoftJoint 동작에 필요한 **파라미터 값을 사전에 설정**하기 위한 명령어입니다.

사용자는 본 명령어를 통해 다음 항목을 설정해야 합니다.
- SoftJoint 적용될 축 번호
- 축의 유연함 정도
- 허용 각도 범위
- 외력 감지를 위한 문턱값

---

### 문법

```plaintext
softjoint_lim, j=<축번호>, sft=<부드러움 정도>, ang=<각도 범위>, thr=<문턱값>
```

---

### 파라미터

| 변수 | 설명 | 범위 / 단위 |
|---------|------|-------------|
| j   | softjoint 기능이 적용될 축 번호 | 로봇 축만 가능 |
| sft | 축의 유연함 정도 (값이 클수록 더 유연하게 동작) | 0: Off, 1 ~ 100 |
| ang | 축의 각도 제한 범위 | degree (deg) |
| thr | 외력 감지를 위한 문턱값 | Nm |

--- 
<br>

{% hint style="info" %}

* `softjoint_lim` 파라미터는 축 번호(`j`)와 부드러움 정도(`sft`)를 필수로 설정해야 합니다.  
* 각도 범위(`ang`)와 문턱값(`thr`)을 설정하지 않을 경우, 각도 제한은 적용되지 않으며 문턱값은 **0.0 Nm**로 자동 설정됩니다.
{% endhint %}

{% hint style="warning" %}
부가축은 해당 기능을 사용할 수 없습니다.  
{% endhint %}
[__SOURCE](3-example/README.md)
# 3. 예시

본 절에서는 SoftJoint 기능의 실제 사용 방법을 이해할 수 있도록  
`softjoint_lim` 및 `softjoint` 명령어를 활용한 **대표적인 설정 및 프로그램 예제**를 제공합니다.

각 예제는 적용 축, 유연함 정도, 각도 제한, 문턱값 등 주요 파라미터 설정에 따른  
SoftJoint 동작 특성을 확인하는 것을 목적으로 하며,  
실제 작업 환경에서의 응용을 고려하여 구성되었습니다.

예제를 통해 다음 사항을 확인할 수 있습니다.
- 단일 축에 SoftJoint를 적용하는 방법
- 복수 축에 서로 다른 파라미터를 적용하는 방법
- SoftJoint 활성화/비활성화 시의 프로그램 흐름
- `delay` 명령어를 포함한 권장 사용 절차
[__SOURCE](3-example/3.1-example.md)
## 3.1 예제 - 3번 축 SoftJoint 파라미터 설정

본 예제는 **3번 관절 축만을 활성화하여**,  
외력에 대해 해당 축이 **지정된 컴플라이언스 특성으로 반응하도록 설정한 기본 사례**입니다.

---

### 설정 개요

3번 축에 대해 **부드러움, 각도 제한, 문턱값을 단일 축 기준으로 설정**하여  
외력에 대한 반응 범위를 명확하게 정의합니다.

---

### 설정 조건

* **활성 축** : 3번 축
* **부드러움 (sft)** : 50 (중간 수준의 컴플라이언스)
* **각도 제한 (ang)** : -30° ~ +30°
* **문턱값 (thr)** : 10 Nm
---

### 프로그램 예제

```python
softjoint_lim j=3, sft=50, ang=30, thr=10
```


[__SOURCE](3-example/3.2-example.md)
## 3.2 예제: 2번·3번 축 SoftJoint 차등 컴플라이언스 설정

본 예제는 **2번 축과 3번 축에 서로 다른 SoftJoint 파라미터를 적용하여**,  
외력 인가 시 두 축이 **각각 상이한 컴플라이언스 특성으로 반응하도록 구성한 사례**입니다.

---

### 설정 개요

각 관절 축에 대해 **부드러움, 각도 제한, 문턱값을 독립적으로 설정**하여  
축별 외력 반응 특성을 세밀하게 제어합니다.

---

### 설정 조건

- **부드러움 (sft)**  
  - 2번 축 : 30 (상대적으로 단단한 반응)  
  - 3번 축 : 80 (보다 유연한 반응)

- **각도 제한 (ang)**  
  - 2번 축 : -50° ~ +50°  
  - 3번 축 : 제한 없음

- **문턱값 (thr)**  
  - 2번 축 : 3 Nm  
  - 3번 축 : 5 Nm

---

### 프로그램 예제

```python
S1   move P, spd=100mm/sec, accu=0, tool=0
     delay 2.0                 # SoftJoint 활성화 전 필수 대기 시간

     softjoint_lim j=2, sft=30, ang=50, thr=3
     softjoint_lim j=3, sft=80, thr=5

     softjoint on

S2   move P, spd=250mm/sec, accu=0, tool=0
     softjoint off
     end
```