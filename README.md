# gitflow-practice

## 이 저장소는 GitFlow 전략을 실무에 적용할 때 발생할 수 있는 8가지 주요 시나리오를 직접 실습해 보기 위해 구성되었습니다. 아래의 시나리오를 순서대로 진행하며 문제를 발생시키고 직접 해결해 보세요.

### 시나리오 1 & 2: 정상 병합과 버전 동기화
1.**[시나리오 2 세팅]** develop 브랜치에서 feature/user-login과 feature/user-logout 두 개의 브랜치를 각각 생성합니다.

2. feature/user-login으로 이동하여 scenario.txt 2번째 줄에 "로그인 기능 완료"를 적고 커밋, 푸시합니다.

3. **[시나리오 1 달성]** GitHub로 이동하여 feature/user-login 브랜치에서 develop 브랜치로 Pull Request(PR)를 열고 Auto-merge를 진행합니다.

4. **[시나리오 2 달성]** 로컬로 돌아와 feature/user-logout 브랜치로 이동합니다. (현재 이 브랜치에는 "로그인 기능 완료"라는 텍스트가 없는 과거 버전 상태입니다.)

5. 아래 명령어를 실행하여 원격의 최신 변경 사항(로그인 기능)을 현재 로컬 브랜치로 안전하게 가져옵니다.

```
Bash
git pull origin develop
```

### 시나리오 3: PR 불가능 상황 연출
1. develop 브랜치에서 feature/empty-test 브랜치를 생성합니다.

2. 아무것도 수정하지 않거나, 파일을 열었다가 그대로 닫고 아래 명령어를 실행합니다.
```
Bash
git push origin feature/empty-test
```
3. GitHub에서 이 브랜치를 develop으로 PR 하려고 시도해 봅니다. 버튼이 비활성화되거나 변경 사항이 없다는 메시지가 뜨는 것을 확인합니다.


## 시나리오 4: PR의 Auto-merge 불가 (Merge Conflict)
1. develop 브랜치를 최신으로 업데이트(git pull) 합니다.

2. feature/a-team과 feature/b-team 브랜치를 각각 생성합니다.

3. feature/a-team에서 scenario.txt의 3번째 줄에 "A팀 작업 완료"라고 적고 커밋, 푸시 후 develop에 PR 및 병합합니다.

4. feature/b-team으로 이동하여 동일한 3번째 줄에 "B팀 작업 완료"라고 적고 커밋, 푸시합니다.

5. GitHub에서 feature/b-team 브랜치를 develop으로 PR 엽니다. (Auto-merge 불가 경고를 확인합니다.)

6. 로컬의 feature/b-team 브랜치에서 아래 명령어를 입력하여 고의로 충돌을 발생시킵니다.

```
git pull origin develop
```
7. 파일의 <<<<<<< 마커를 지우고 두 팀의 코드를 알맞게 수정한 뒤, 다시 커밋하고 푸시하여 PR 창이 초록색으로 변하는 것을 확인하고 병합합니다.

## 시나리오 5: Hotfix (긴급 배포)
1. 지금 당장 develop에는 아직 배포 안 된 B팀의 코드가 섞여 있다고 가정합니다.

2. main 브랜치로 이동하여 코드를 최신화합니다.

```
git checkout main
git pull origin main
```
3. main에서 hotfix/urgent-bug 브랜치를 생성합니다.

4. scenario.txt 첫 줄 "GitFlow 실습 시작" 옆에 "(긴급 수정됨)"을 추가하고 커밋, 푸시합니다.

5. GitHub에서 이 브랜치를 main으로 1번, develop으로 1번 각각 PR을 열어 병합합니다.

## 시나리오 6: Release QA 버그 수정
1. develop 브랜치로 이동 후 release/v1.0 브랜치를 생성합니다. (배포 준비 완료 선언)

2. 배포 직전 테스트 중 오타를 발견했다고 가정합니다. develop이 아닌 release/v1.0 브랜치에서 직접 scenario.txt 하단에 "오타 수정 완료"를 적고 커밋, 푸시합니다.

3. release/v1.0 브랜치를 main과 develop에 각각 PR 하여 병합합니다.


## 시나리오 7: Revert (병합 취소)
1. develop 브랜치에서 feature/bad-code 브랜치를 생성합니다.

2. scenario.txt 전체를 지워버리고 "치명적 버그 발생"이라고 적은 뒤 커밋, 푸시합니다.

3. develop으로 PR을 열고 병합합니다. (의도적인 문제 발생)

4. GitHub의 develop 브랜치 커밋 기록을 보고, 방금 병합된 Merge Commit의 해시값(예: abc1234)을 찾습니다.

5. 로컬 develop 브랜치를 최신화한 뒤, 아래 명령어를 입력하여 악성 코드가 들어오기 전 상태로 되돌리는 커밋을 만들고 푸시합니다.

```
git revert -m 1 [커밋 해시값]
git push origin develop
```

## 시나리오 8: Feature 폐기
1. develop 브랜치에서 feature/cancel-test 브랜치를 만듭니다.

2. 작업을 진행하고 푸시한 뒤 PR까지 생성합니다.

3. 기획이 취소되었다고 가정하고, PR 하단의 Close pull request를 눌러 PR을 닫습니다.

4. 로컬 터미널에서 아래 명령어로 로컬 및 원격 브랜치를 깔끔하게 삭제합니다.

```
git branch -D feature/cancel-test
git push origin --delete feature/cancel-test
```

