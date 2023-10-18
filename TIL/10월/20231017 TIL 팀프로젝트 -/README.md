# 2023.10.17 TIL | 팀프로젝트 - 

[팀프로젝트](https://github.com/tjdgh7419/Chapter3-3_B07_Project)

[팀프로젝트 - 1](https://github.com/KimMaYa1/NBC/tree/main/TIL/10%EC%9B%94/20231013%20TIL%20%ED%8C%80%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%20%EC%8B%9C%EC%9E%91)
[팀프로젝트 - 2](https://github.com/KimMaYa1/NBC/tree/main/TIL/10%EC%9B%94/20231016%20TIL%20%ED%8C%80%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%20-%20%EC%B2%B4%EB%A0%A5%EB%B0%94%20%EC%84%9C%EC%84%9C%ED%9E%88%20%EC%A4%84%EC%9D%B4%EA%B8%B0)

## 오늘 한 일

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