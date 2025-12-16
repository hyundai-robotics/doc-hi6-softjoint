# 🧩 2. 명령어

SoftJoint 기능은 두 개의 명령어(`softjoint_lim`, `softjoint`)를 통해 설정 및 제어됩니다.  

- `softjoint_lim` 명령어는 SoftJoint 동작에 필요한 **기본 파라미터를 사전에 정의**하는 역할을 합니다.
- `softjoint` 명령어는, `softjoint_lim`에서 설정된 파라미터를 기준으로 **SoftJoint 기능을 활성화 또는 비활성화**합니다.

따라서 SoftJoint 기능을 사용하기 위해서는,  
반드시 `softjoint_lim` 명령어를 먼저 사용하여 적용할 축과 동작 특성을 설정한 후  
`softjoint on` 명령어를 통해 기능을 활성화해야 합니다.

`softjoint_lim` 명령어를 통해 사용자는 다음과 같은 항목을 정의할 수 있습니다.
- softjoint가 적용될 축 번호
- 외력에 대한 반응의 부드러움 정도
- 축의 허용 각도 범위
- 외력 감지를 위한 문턱값