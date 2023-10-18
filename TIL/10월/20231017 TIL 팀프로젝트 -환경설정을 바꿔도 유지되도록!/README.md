# 2023.10.17 TIL | 팀프로젝트 - 환경설정을 바꿔도 유지되도록!

[팀프로젝트](https://github.com/tjdgh7419/Chapter3-3_B07_Project)

[팀프로젝트 - 1](https://github.com/KimMaYa1/NBC/tree/main/TIL/10%EC%9B%94/20231013%20TIL%20%ED%8C%80%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%20%EC%8B%9C%EC%9E%91)
[팀프로젝트 - 2](https://github.com/KimMaYa1/NBC/tree/main/TIL/10%EC%9B%94/20231016%20TIL%20%ED%8C%80%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%20-%20%EC%B2%B4%EB%A0%A5%EB%B0%94%20%EC%84%9C%EC%84%9C%ED%9E%88%20%EC%A4%84%EC%9D%B4%EA%B8%B0)

## 오늘 한 일

### 환경설정 UI

<details>
<summary>코드</summary>

  ```C#
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;

public class PreferencesPanel : StartUIBase
{
    [SerializeField] private Button graphicButton;
    [SerializeField] private Button audioButton;

    [Header("Panel")]
    [SerializeField] private GameObject graphicPanel;
    [SerializeField] private GameObject audioPanel;

    [Header("Save")]
    [SerializeField] private SaveSetting saveSetting;

    private void Start()
    {
        graphicButton.onClick.AddListener(OnGraphicPanel);
        audioButton.onClick.AddListener(OnAudioPanel);
        saveSetting.CallAudioSetting();
        saveSetting.CallGraphicSetting();
    }

    protected override void Close()
    {
        if (SceneManager.GetActiveScene().name == "StartScene")
        {
            base.Close();
        }
        if (SceneManager.GetActiveScene().name == "MainScene")
        {
            gameObject.SetActive(false);
            UIManager.Instance.OpenUI<PausePanel>();
            SoundManager.Instance.EffactMusic.Click1SoundPlay();
        }
        saveSetting.CallAudioSetting();
        saveSetting.CallGraphicSetting();
    }

    private void OnGraphicPanel()
    {
        graphicPanel.SetActive(true);
        audioPanel.SetActive(false);
        SoundManager.Instance.EffactMusic.Click2SoundPlay();
    }
    
    private void OnAudioPanel()
    {
        graphicPanel.SetActive(false);
        audioPanel.SetActive(true);
        SoundManager.Instance.EffactMusic.Click2SoundPlay();
    }
}

  ```
</details>

- 환경설정 UI가 닫혀도 게임신에서는 일시정지 패널이기때문에 Close를 오버라이드 하여 사용하였다.
- 이외에 값을 변경후 저장 버튼을 눌렀을때 사운드 매니저나 나중에 추가할 그래픽 매니저를 위해 세이브 셋팅 스크립트를 따로 만들었다

### 세이브 스크립트

<details>
<summary>코드</summary>

  ```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class SaveSetting : MonoBehaviour
{
    [SerializeField] private Button saveButton;

    [Header("Audio")]
    [SerializeField] protected Slider MasterSlider;
    [SerializeField] protected Slider MusicSlider;
    [SerializeField] protected Slider EffactSlider;

    [Header("Graphic")]
    [SerializeField] private Toggle EffactToggle;
    [SerializeField] private Toggle ShadowToggle;

    private void Start()
    {
        saveButton.onClick.AddListener(OnPreferencesSave);
        saveButton.onClick.AddListener(() => SoundManager.Instance.EffactMusic.Click2SoundPlay());
        OnPreferencesSave();
    }

    public void OnPreferencesSave()
    {
        SoundManager.Instance.SetAudioSetting(MasterSlider.value, MusicSlider.value, EffactSlider.value);
        UIManager.Instance.SetGraphicSetting(EffactToggle.isOn, ShadowToggle.isOn);
    }

    public void CallAudioSetting()
    {
        float[] audios = SoundManager.Instance.GetAudioSetting();
        MasterSlider.value = audios[0];
        MusicSlider.value = audios[1];
        EffactSlider.value = audios[2];
    }

    public void CallGraphicSetting()
    {
        bool[] graphic = UIManager.Instance.GetGraphicSetting();
        EffactToggle.isOn = graphic[0];
        ShadowToggle.isOn = graphic[1];
    }
}

  ```
</details>

- 바를 조정하였을때 세이브 버튼을 누르면 OnPreferencesSave를 사용하여 값을 사운드 매니저와 나중에 생길 그래픽 매니저(현재는 UI매니저가 가지고있음)에 전달하여주었다.
- CallAudioSetting / CallGraphicSetting 함수들은 씬이 이동하였을때도 값을 불러와 환경설정 창을 열었을때 값이 똑같이 유지되도록 하였다.

### 사운드 매니저

<details>
<summary>코드</summary>

  ```C#
 using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class SoundManager : MonoBehaviour
{
    public static SoundManager Instance;

    public BGM BackMusic;
    public EM EffactMusic;

    private AudioSource BGM;
    private AudioSource EM;

    [Range(0f, 1f)] private static float MasterVolume = 1;
    [Range(0f, 1f)] private static float MusicVolume = 1;
    [Range(0f, 1f)] private static float EffactVolume = 1;

    private void Awake()
    {
        Instance = this;

        BGM = BackMusic.GetComponent<AudioSource>();
        EM = EffactMusic.GetComponent<AudioSource>();

        BGM.volume = MasterVolume * MusicVolume;
        EM.volume = MasterVolume * EffactVolume;
    }

    public void SetAudioSetting(float master, float music, float efffact)
    {
        MasterVolume = master;
        MusicVolume = music;
        EffactVolume = efffact;

        BGM.volume = MasterVolume * MusicVolume;
        EM.volume = MasterVolume * EffactVolume;
    }

    //0번 마스터볼륨 1번 뮤직볼륨 2번 이펙트볼륨
    public float[] GetAudioSetting()
    {
        return new float[] { MasterVolume, MusicVolume, EffactVolume };
    }
}

  ```
</details>

- 환경설정에서 설정한 데이터들을 저장하여 가지고 있다가 씬이 넘어가도 소리가 유지될수있도록 관리해주는 사운드 매니저이다.
- 사실 사운드 매니저는 씬을 옮겨도 사라지지 않게 할수있지만 게임씬에서만 사용하는 게임매니저와 스타트씬과 게임씬에서 다른 UI매니저가 있어서 다시 선언하는것으로 하였다.
- 다음에는 각 매니저들을 씬에서 필요에 따라 불러올수있도록 바꾸고싶다.

### 할것들
- 검기 이펙트도 만들어야하고 퀘스트... 각종 상황에 따른 사운드를 찾고 구현하고 다 배정해줘야한다 팀원들과 커뮤니티가 중요할거같은 내일이다... 화이팅!!