# 던전 오디세이 장비 합성 비용 계산기

목표 장비 하나를 만들 때까지 필요한 **소환 횟수와 다이아 소모량**을 몬테카를로 시뮬레이션으로 추정합니다.

👉 **https://ohjih.github.io/dungeon-calc/**

한국어 / English 두 언어를 지원합니다. 페이지 오른쪽 위 버튼으로 바꾸면 선택이 저장되고,
`설정 링크 복사`로 만든 주소에도 언어가 함께 담깁니다 (`?lang=ko` / `?lang=en`).

*Available in English — use the toggle at the top right, or open
[?lang=en](https://ohjih.github.io/dungeon-calc/?lang=en) directly. Scroll down for the English summary.*

## 쓰는 법

1. 위 탭에서 장비 종류(무기 / 마법서 / 반지 / 탈리스만)를 고릅니다.
2. **상점 레벨**과 현재 레벨에서 이미 뽑은 횟수를 입력합니다.
3. 게임 화면의 `n/5` 숫자를 보고 등급별 **보유 수량**을 채웁니다.
   장착 중인 장비는 이 숫자에 안 잡히니, 재료로 쓸 거면 해당 칸에 +1 해주세요.
4. 만들고 싶은 칸을 눌러 **목표**로 지정하고 `계산하기`.

`설정 링크 복사`를 누르면 지금 입력값이 URL에 담겨서, 그 링크로 다시 열면 그대로 복원됩니다.

## 계산 방식

- 합성은 **같은 등급·같은 성급 5개 → 다음 성급 1개**, 4성 5개 → 상위 등급 1성.
  손실이 없으므로 한 단계 오를 때마다 가치가 정확히 5배입니다.
- D\~A 등급은 자주 나오고 값이 작아 **기댓값(평균)** 으로 처리하고,
  S/SS 등급은 드물고 값이 커서 **기하분포로 개별 추첨**합니다.
- 상점 레벨은 뽑은 횟수에 따라 자동으로 올라가며, 레벨마다 확률표가 바뀝니다.

## 알려진 한계

- **합성 잉여(5개를 못 채운 나머지)를 무시**합니다. 중앙값이 실제보다 최대 3%가량 낙관적입니다.
  목표가 클수록(SS 이상) 오차는 작아집니다.
- D\~A를 평균으로 처리하므로 **S 1\~2성처럼 값싼 목표에서는 90% 구간 위쪽이 좁게 나옵니다.**
  실제로는 운이 나쁘면 더 걸릴 수 있습니다. SS 이상 목표에서는 영향이 없습니다.
- **레벨업 요구 소환수는 Lv4→5부터 Lv12→13까지 실측값**입니다(고급 설정에 `실측`으로 표시).
  그 바깥은 추세로 채운 추정값입니다.
  - `Lv1→2 ~ Lv3→4` 추정은 **상점 레벨 4 이상에서 시작하면 쓰이지 않습니다.**
    Lv4 미만에서 시작하면 오차가 커집니다(S 4★ 기준 3,700\~6,600회 사이에서 흔들림).
  - `Lv13→14`, `Lv14→15` 추정은 결과를 잡음 수준(0\~2%)으로만 움직여 무시해도 됩니다.

## 확인된 전제

등급·별 확률표는 **게임 내 확률 공개표에서 직접 옮겨 적고 검토를 마친 값**입니다.
**4개 카테고리의 확률이 모두 같다는 것**과 **천장(pity)이 없다는 것**도 확인했습니다.

자세한 확률표와 검증 기준값은 [HANDOVER.md](HANDOVER.md)에 있습니다.


---

# Dungeon Odyssey — Equipment Fusion Cost Calculator

Estimates how many summons and diamonds it takes to build one target item, using a Monte Carlo simulation.

👉 **https://ohjih.github.io/dungeon-calc/?lang=en**

## How to use

1. Pick an equipment type at the top (Weapon / Spellbook / Ring / Talisman).
2. Enter your **shop level** and how many summons you have already made at that level.
3. Fill in the **owned counts** by copying the `n/5` numbers from the game screen.
   Equipped gear does not seem to be counted there — if you can use it as material, add 1 to that cell.
4. Tap the cell you want to build to make it the **target**, then press `Calculate`.

`Copy setup link` puts your current inputs into the URL, so reopening that link restores everything.

## How it works

- Fusion is **5 of the same grade and star → 1 of the next star**, and 5× a 4-star → 1 of the next grade at 1 star.
  Nothing is lost, so each step up is worth exactly 5×.
- Grades D\~A are frequent and low-value, so they are handled by **expected value**.
  S and SS are rare and high-value, so they are **drawn individually** from a geometric distribution.
- Shop level rises automatically with summon count, and each level has its own probability table.

## Known limits

- **Fusion remainders (leftovers that never reach 5) are ignored.** The median runs up to ~3% optimistic.
  The bigger the target (SS and above), the smaller the error.
- Because D\~A are averaged, **the upper half of the 90% range is too narrow for cheap targets like S 1\~2 star.**
  A bad run can really take longer. This does not affect SS-tier goals.
- **Level-up requirements are measured in game from Lv4→5 through Lv12→13** (tagged `MEASURED` under `Advanced`).
  Everything outside that range is extrapolated from the measured trend.
  - The `Lv1→2 ~ Lv3→4` estimates are **never used if you start at shop level 4 or higher.**
    Below that they add real error (an S 4-star goal ranges 3,700\~6,600 summons depending on the guess).
  - The `Lv13→14` and `Lv14→15` estimates move results only within noise (0\~2%), so they are safe to ignore.

## Confirmed assumptions

The grade and star probability tables were transcribed directly from the game's published drop-rate screen
and checked. All four categories share the same rates, and there is **no pity system**.
