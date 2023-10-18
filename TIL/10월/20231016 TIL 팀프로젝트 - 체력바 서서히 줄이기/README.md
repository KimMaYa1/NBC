# 2023.10.16 TIL | 팀프로젝트 - 체력바 서서히 줄이기

[팀프로젝트](https://github.com/tjdgh7419/Chapter3-3_B07_Project)

[팀프로젝트 - 1](https://github.com/KimMaYa1/NBC/tree/main/TIL/10%EC%9B%94/20231013%20TIL%20%ED%8C%80%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%20%EC%8B%9C%EC%9E%91)

## 오늘 한 일

### 체력바 서서히 줄이기
<details>
<summary>코드</summary>

  ```C#
  using System.Collections;
using TMPro;
using UnityEngine;
using UnityEngine.UI;

public class PlayerUI : MonoBehaviour
{
    [Header("PlayerCondition")]
    [SerializeField] private PlayerConditionManager PCM;

    [Header("HP")]
    [SerializeField] private Image ReducedHPBar;
    [SerializeField] private Image CurrentHPBar;
    [SerializeField] private TextMeshProUGUI HPText;

    [Header("MP")]
    [SerializeField] private Image ReducedMPBar;
    [SerializeField] private Image CurrentMPBar;
    [SerializeField] private TextMeshProUGUI MPText;

    private float maxHp;
    private float maxMp;
    private float currentHp;
    private float currentMp;

    // Start is called before the first frame update
    void Start()
    {
        Cursor.visible = false;
        Cursor.lockState = CursorLockMode.Locked;

        maxHp = PCM.hp.maxValue;
        maxMp = PCM.mp.maxValue;
        currentHp = PCM.hp.curValue;
        currentMp = PCM.mp.curValue;

        UpdateBar();
    }

    private void UpdateBar()
    {
        HPText.text = $"{currentHp}/{maxHp}";
        MPText.text = $"{currentMp}/{maxMp}";
    }

    public void TakeDamage(float _damage, string type = null)
    {
        if (type == "MP")
        {
            if (currentMp < _damage)
            {
                _damage = currentMp;
            }
            currentMp -= _damage;
        }
        else
        {
            if (currentHp <= _damage)
            {
                _damage = currentHp;
                UIManager.Instance.OpenUI<GameOver>();
            }
            currentHp -= _damage;
        }
        StartCoroutine(TakeDamage(type));
    }

    public void Healing(float _heal, string type = null)
    {
        if (type == "MP")
        {
            if (currentMp + _heal >= maxMp)
            {
                _heal = maxMp - currentMp;
            }
            currentMp += _heal;
        }
        else
        {
            if (currentHp + _heal >= maxHp)
            {
                _heal = maxHp - currentHp;
            }
            currentHp += _heal;
        }
        StartCoroutine(Healing(type));
    }

    IEnumerator TakeDamage(string type = null)
    {
        yield return null;
        UpdateBar();

        if (type == "MP")
        {
            CurrentMPBar.fillAmount = currentMp / maxMp;

            while (true)
            {
                yield return null;
                if (ReducedMPBar.fillAmount <= CurrentMPBar.fillAmount + 0.0001f)
                {
                    ReducedMPBar.fillAmount = CurrentMPBar.fillAmount;
                    yield break;
                }
                ReducedMPBar.fillAmount = Mathf.Lerp(ReducedMPBar.fillAmount, currentMp / maxMp, Time.deltaTime * 3);
            }
        }
        else
        {
            CurrentHPBar.fillAmount = currentHp / maxHp;

            while (true)
            {
                yield return null;

                if (ReducedHPBar.fillAmount <= CurrentHPBar.fillAmount + 0.0001f)
                {
                    ReducedHPBar.fillAmount = CurrentHPBar.fillAmount;
                    yield break;
                }
                ReducedHPBar.fillAmount = Mathf.Lerp(ReducedHPBar.fillAmount, currentHp / maxHp, Time.deltaTime * 3);
            }
        }
    }

    IEnumerator Healing(string type = null)
    {
        yield return null;
        UpdateBar();
        
        if (type == "MP")
        {
            ReducedMPBar.fillAmount = currentMp / maxMp;

            while (true)
            {
                yield return null;
                if (ReducedMPBar.fillAmount <= CurrentMPBar.fillAmount + 0.0001f)
                {
                    CurrentMPBar.fillAmount = ReducedMPBar.fillAmount;
                    yield break;
                }
                CurrentMPBar.fillAmount = Mathf.Lerp(CurrentMPBar.fillAmount, currentMp / maxMp, Time.deltaTime * 3);
            }
        }
        else
        {
            ReducedHPBar.fillAmount = currentHp / maxHp;

            while (true)
            {
                yield return null;
                if (ReducedHPBar.fillAmount <= CurrentHPBar.fillAmount + 0.0001f)
                {
                    CurrentHPBar.fillAmount = ReducedHPBar.fillAmount;
                    yield break;
                }
                CurrentHPBar.fillAmount = Mathf.Lerp(CurrentHPBar.fillAmount, currentHp / maxHp, Time.deltaTime * 3);
            }
        }
    }
}

  ```
</details>

- PlayerUI를 UIManager에 저장된 딕셔너리에서 받아서 TakeDamge 나 Healing을 이용할수 있다.
- 지금 넣어둔 기능들은 따로 스크립트를 만들어 사용하는 법을 찾아야 할거같다.
- 플레이어의 체력은 다른 동료분이 구현하신 PlayerConditionManager에 존재합니다.
- 솔직히 이 기능은 만들고 나면 게임이 뭔가 있어보이기 때문에 자주 사용할거같다.

### 일시정지 하기

<details>
<summary>코드</summary>

  ```C#
using UnityEngine;
using UnityEngine.UI;

public class PausePanel : GameUIBase
{
    [Header("Buttons")]
    [SerializeField] private Button continueButton;
    [SerializeField] private Button quitButton;
    [SerializeField] private Button preferencesButton;

    private void Start()
    {
        continueButton.onClick.AddListener(Close);
        quitButton.onClick.AddListener(OpenUI_Quit);
        preferencesButton.onClick.AddListener(OpenUI_Preferences);
    }

    void OpenUI_Quit()
    {
        var uiPopUp = UIManager.Instance.OpenUI<UIPopUp>();
        uiPopUp.SetAction("나가기","메인화면으로 돌아가시겠습니까?\n(현재까지의 정보가 모두 사라집니다)",() => LoadSceneManager.LoadScene("StartScene"));
        
    }
    void OpenUI_Preferences()
    {
        UIManager.Instance.OpenUI<PreferencesPanel>();
        Close();
    }
}
  ```
</details>

- OpenUI_Quit 에서는 나가기 버튼을 눌렀을때 바로 나가지면 정이 없으니.. 사실 저장 기능이 따로 없어서 나가면 데이터가 다 사라지기 때문에 다시한번 물어보는 팝업을 띄워준다.
- OpenUI_Preferences는 말그대로 환경설정 UI를 띄워준다

### 공용 UIPopUp

<details>
<summary>코드</summary>

  ```C#
using System;
using TMPro;
using UnityEngine;
using UnityEngine.UI;

public class UIPopUp : GameUIBase
{
    [SerializeField] private Button cheackButton;

    [Header ("Info")]
    [SerializeField] private TextMeshProUGUI headingText;
    [SerializeField] private TextMeshProUGUI explanationText;

    private Action OnConfirm;
    private Action UnConfirm;

    protected virtual void Awake()
    {
        base.Awake();
        cheackButton.onClick.AddListener(Confirm);
        closeButton.onClick.AddListener(NotConfirm);
    }

    public void SetAction(string _headingText, string _explanationText, Action onConfirm = null, Action unConfirm = null)
    {
        headingText.text = _headingText;
        explanationText.text = _explanationText;

        OnConfirm = onConfirm;
        UnConfirm = unConfirm;
    }

    void Confirm()
    {
        /*if (OnConfirm != null)
        {
            OnConfirm();
            OnConfirm = null;
        }*/
        OnConfirm?.Invoke();
        OnConfirm = null;

        Close();
    }

    void NotConfirm()
    {
        OnCheackButton();
        Close();
        UnConfirm?.Invoke();
        UnConfirm = null;

    }

    public void OffCheackButton()
    {
        if (cheackButton.gameObject.active)
        {
            cheackButton.gameObject.SetActive(false);
        }
    }
    
    public void OnCheackButton()
    {
        if (!cheackButton.gameObject.active)
        {
            cheackButton.gameObject.SetActive(true);
        }
    }
}

  ```
</details>

- SetAction으로 팝업 제목, 내용을 받고 그뒤 순서대로 확인 , 닫기 순으로 확인만 필용한경우 닫기만 필요한 경우 (제작시에는 닫기버튼을 누르면 다시 제작으로 돌아가기 때문에 커서가 잠기면 안된다) 가 다르기 때문에 위처럼 만들었다
- 변수를 받을때 null값을 줘서 다른곳에서 함수를 선언할때 파라미터를 전해주지 않아도 오류가 안나오도록 하였다. (단점 어디선가 잘못된 정보를 주는데도 어디가 잘못된거지 바로 확인하지 못함) (그래도 장점이 더 많은거 같아서 그냥 쓸거임 ㅋㅋ)

### 이외 할것들

- 환경설정 UI 기능을 구현해야한다! 그리고 퀘스트도;; (사운드...이펙트...) 할거 겁나 많아 ㅜㅜ