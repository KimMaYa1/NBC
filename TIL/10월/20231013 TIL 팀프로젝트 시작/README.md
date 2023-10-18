# 2023.10.13 TIL | 팀프로젝트시작

[팀프로젝트](https://github.com/tjdgh7419/Chapter3-3_B07_Project)

## 팀프로젝트 역활

- 나는 요번 프로젝트에서 맡아보지 않았던 UI를 맡아보게되었다. 
한번도 맡아 보지 않았던 역활이라 새로운것을 배울수 있을거같아 기쁜상태입니다!

## 오늘 한 일

### UIManger구성
<details>
<summary>코드</summary>

  ```C#
  using System;
  using System.Collections.Generic;
  using UnityEngine;
  using UnityEngine.SceneManagement;

  public class UIManager : MonoBehaviour
  {
    public static UIManager Instance;

    [Header("Shadow")]
    public static bool Effact = true;
    public static bool Shadow = true;

    private Dictionary<string, GameObject> _uiList = new Dictionary<string, GameObject>();
    private void Awake()
    {
        Instance = this;

        InitUIList();
    }

    private void InitUIList()
    {
        int uiCount = transform.childCount;

        for (int i = 0; i < uiCount; i++)
        {
            Transform go = transform.GetChild(i);
            _uiList.Add(go.name, go.gameObject);
            go.gameObject.SetActive(false);
        }
    }

    public T OpenUI<T>()
    {
        var obj = _uiList[typeof(T).Name];
        obj.SetActive(true);
        return obj.GetComponent<T>();
    }

    public void SetGraphicSetting(bool _effact, bool _shadow)
    {
        Effact = _effact;
        Shadow = _shadow;
    }

    //0번 이펙트 1번 쉐도우
    public bool[] GetGraphicSetting()
    {
        return new bool[] { Effact, Shadow};
    }
  }

  ```

</details>
- 이 매니저 스크립트로 자식들을 전부 Dictionary 안에 저장해 필요할때 OpenUI메서드를 사용하여 열수있도록 만들었다.

### 로딩씬 구성
<details>

<summary>코드</summary>
  
  ```C#
  using System.Collections;
using TMPro;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;

public class LoadSceneManager : MonoBehaviour
{
    public static string nextScene;

    [SerializeField] Image progressBar;
    [SerializeField] TextMeshProUGUI progressText;

    void Start()
    {
        StartCoroutine(LoadScene());
    }

    public static void LoadScene(string _nextScene)
    {
        Time.timeScale = 1.0f;
        nextScene = _nextScene;
        SceneManager.LoadScene("LoadingScene");
    }

    IEnumerator LoadScene()
    {
        yield return null;

        AsyncOperation op = SceneManager.LoadSceneAsync(nextScene);     //비동기화로 다음씬 불러오기
        op.allowSceneActivation = false;            //불러 오는 씬 안보이게

        float timer = 0f;

        while (!op.isDone)      //불러오는 씬이 전부 불려오지 않았다면 반복
        {
            yield return null;

            timer += Time.deltaTime;
			
			if (op.progress < 0.9f)      //불러온 정도가 90% 아래라면
            {
                progressBar.fillAmount = Mathf.Lerp(progressBar.fillAmount, op.progress, timer);
                progressText.text = $"로딩중...{(int)(progressBar.fillAmount * 100)}%";
                if (progressBar.fillAmount >= op.progress)
                {
                    timer = 0f;
                }
            }
            else if (timer > 1)
            {
                progressBar.fillAmount = Mathf.Lerp(progressBar.fillAmount, 1, timer);
                progressText.text = $"로딩중...{(int)(progressBar.fillAmount * 100)}%";

                if (progressBar.fillAmount == 1f)
                {
                    op.allowSceneActivation = true;
                    yield break;
                }
            }
            else
            {
                progressBar.fillAmount = 0.9f;
                progressText.text = "로딩중...90%";
            }
        }
    }
}

  ```
</details>
- 내 TIL에 있는 로딩씬 구성방법에서 조금더 추가하여 구성하였습니다.
- 나중에는 씬교체 방식이 아닌 페이드 인 방식으로 만들어 보고 싶습니다.

### 이외 패널 UI 구성

- 체력바 / 일시정지창 / 환경설정 창의 UI를 구성하였다 UI들은 전부 손수 제작하였습니다 >_0

### 앞으로 할 것

- 만들었던 UI들의 기능을 구현하고 오디오 & 그래픽의 연결을 해야겠다.