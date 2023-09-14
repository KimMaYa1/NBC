# 내일배움캠프 20일차 TIL | 3주차 팀프로젝트완성 이후에 개발은?

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

[팀과제 링크](https://github.com/phw97123/B10_DreamsComeTrue)

[팀과제 시연 영상](https://www.youtube.com/watch?v=3ZMRb3Ro87o)

# 오늘 알게된 메서드

## 플레이어 스턴시 깜빡이기

[플레이어 컨트롤러 스크립트](https://github.com/phw97123/B10_DreamsComeTrue/blob/main/Assets/Scripts/PlayerMove/PlayerController.cs)

### 1. 문제

  ```
  IEnumerator StunEffect()
  {
    // WaitForSeconds 객체를 미리 생성하고 변수에 할당하여 가비지 생성을 최소화 
    WaitForSeconds waitTime = new WaitForSeconds(0.2f);
    for (int i = 0; i <= 2; i++)
    {
        playerSpriteRenderer.color = Color.gray;
        yield return waitTime;
        playerSpriteRenderer.color = Color.white;
        yield return waitTime;
    }
  }

  public void BugDieItemCount()
  {
    if (BugDieItem.activeSelf == true)
    {
        BugDie -= 1;
        BugDieItemCountTxt.text = BugDie.ToString();
        if (BugDie == 0)
        {
            BugDie = 16;
            BugDieItem.SetActive(false);
        }
    }
    else
    {
        GetComponent<PlayerInput>().enabled = false;
        _isStun = true;

        StartCoroutine(StunEffect()); 
    }
  }
  ```

- 해당 코드는 플레이어가 나쁜벌레를 맞게되면 스턴을 당하면 깜빡이게하는 코드입니다 해당 코드에서는 스턴중에 한번더 나쁜벌레 맞게되면 스턴이 풀리고도 깜빡이는 현상이 일어남
- 해당 코드에서는 waitTime을 위에서 먼저 초기화해주었는데 스턴 이펙트가 진행될때마다 for문 안에서 new를 사용하여 시간을 기다리게 한다면 가비지데이터가 쌓이기때문에 그것을 방지하기위해서 사용하엿습니다.

### 2. 예상 해결 방안

  ```
  IEnumerator StunEffect()
  {
    // WaitForSeconds 객체를 미리 생성하고 변수에 할당하여 가비지 생성을 최소화 
    WaitForSeconds waitTime = new WaitForSeconds(0.2f);
    for (int i = 0; i <= 2; i++)
    {
        if(!_isStun)
            waitTime = 0;
        playerSpriteRenderer.color = Color.gray;
        yield return waitTime;
        if(!_isStun)
            waitTime = 0;
        playerSpriteRenderer.color = Color.white;
        yield return waitTime;
    }
  }
  ```

- 이렇게 스턴을 판단하는 _isStun의 bool값을 가져와 false라면 waitTime을 0으로 바꾸어 바로 빠져나가면 될거같습니다 
- break를 사용하여 바로 빠져나가도 되지만 break문의 사용시 생각치 못한 오류가 생길수있어 사용하지 않았습니다.

## 향후 알고 싶은 것들

- 다른팀들의 프로젝트를 보면서 한가지 눈에 띄었던것이 로딩씬이었습니다. 제가 초반 프로젝트 기획을 할때 로딩씬도 들어가면 좋겠다고 생각했는데 시간과 능력의 부족으로 프로젝트 제출전에 로딩씬을 만들지 못하였습니다. 그래도 시간이 괜찮다면 로딩씬을 만들어서 프로젝트에 추가해보고 그렇지 못한다면 로딩씬을 배워서 다음 프로젝트에 적용해볼 생각입니다.