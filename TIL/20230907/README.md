# 내일배움캠프 15일차 TIL | 유니티 입문 - 개인과제

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

## 개인과제 링크
<htr>https://github.com/KimMaYa1/Unity

## 개인과제 시연영상
<htr>https://www.youtube.com/watch?v=DK2LrI6F9pM

## 깃 허브 대용량

오늘 개인과제를 마치고 깃허브에 유니티 파일을 올리는 도중 유니티 파일의 용량이 커서 올라가지 못하는 사태를 마주했는데 동료분들이 LFS를 사용하면 대용량 파일을 올릴수있다고 하여 찾아보았지만 아무리 적용을 해보려고 해도 적용 되지 않았습니다.
나중에 알고보니 이그노어파일의 위치가 올바르지 않아 적용되지 않았던 거였습니다

![Alt text](image.png)
![Alt text](image-1.png)

- 위에 사진들처럼 유니티 파일 내부에 gitignore를 넣어야하는데 밖에다가 두어서 적용이 안되었습니다 잘 숙지해두고 앞으로는 이런 사태가 없기를!

## 유니티 개인과제

### 오늘 구현하고 추가된 메서드

  ```
    public GameObject[] _npcs;
    private GameObject _taklUI;
    private GameObject _taklBox;
    private Text _npcName;
    private Text _time;
    private bool _onChat = true;

    void Start()
    {
        _npcs = GameObject.FindGameObjectsWithTag("Character");
        nameInputCollection = Canvas.transform.Find("NameInputCollection").gameObject;
        _inputName = nameInputCollection.transform.Find("NameInput").gameObject.transform.Find("inputNameText").gameObject.GetComponent<Text>();
        _nameErrorText = nameInputCollection.transform.Find("NameErrorText").gameObject;
        _select = nameInputCollection.transform.Find("Select").gameObject;
        _uI = Canvas.transform.Find("UI").gameObject;
        _taklUI = Canvas.transform.Find("TalkUI").gameObject;
        _taklBox = Canvas.transform.Find("TalkBox").gameObject;
        _attendPepleWindow = _uI.transform.Find("AttendPepleWindow").gameObject;
        _time = _uI.transform.Find("Time").gameObject.GetComponent<Text>();
        _attendPepleName = _attendPepleWindow.transform.Find("AttendPepleNames").gameObject.GetComponent<Text>();
        _npcName = _taklUI.transform.Find("NpcName").gameObject.GetComponent<Text>();
        _nameErrorText.SetActive(false);
        _attendPepleWindow.SetActive(false);
        _uI.SetActive(false);
        _taklUI.SetActive(false);
        _taklBox.SetActive(false);
    }

    void Update()
    {
        if (_player.active)
        {
            TimeView();
            NpcTextBox();
            AttendListAdd();
        }
    }

    private void NpcTextBox()
    {
        float[] dist = new float[_npcs.Length];
        int count = 0;
        int num = 0;
        foreach (GameObject gameObject in _npcs)
        {
            dist[count++] = Vector3.Distance(_player.transform.position, gameObject.transform.position);
        }
        float min = dist[0];
        for (int i = 0; i < dist.Length; i++)
        {
            if(min > dist[i])
            {
                min = dist[i];
                num = i;
            }
        }
        if(min <= 5 && _onChat)
        {
            _taklUI.SetActive(true);
            _npcName.text = _npcs[num].GetComponentInChildren<Text>().text;
        }
        else if (min > 5)
        {
            _onChat = true;
            _taklUI.SetActive(false);
            _taklBox.SetActive(false);
        }
    }

    private void TimeView()
    {
        DateTime dateTime = DateTime.Now;
        _time.text = dateTime.Hour + ":" + dateTime.Minute;
    }

    public void OnTalk()
    {
        _onChat = false;
        _taklUI.SetActive(false);
        _taklBox.SetActive(true);
    }

    public void OffTalk()
    {
        _onChat = true;
        _taklBox.SetActive(false);
    }
  ```

  - strat부분에서는 필드 메서드들을 추가해주는 부분인데 필드에 더 많이 있지만 오늘 새롭게 추가된거만 작성하였습니다
  - update부분에서는 계속해서 반영되어야하는 부분들을 반영시키는데 메서드로 호출하는것이 더 이해하기 쉬워서 NpcTextBox 와 TimeView 메서드를 추가하였습니다.
  - NpcTextBox 메서드는 거리 계산을 해서 텍스트를 띄워주고 만약 대화를 시작했다면 제일 하단의 OnTalk / OffTalk 메서드를 호출하여 계속해서 뜨거나 멀어졌다가 다시 붙었을때를 관리하였습니다.
  - TimeView 메서드는 말그대로 시간을 표시해주는 메서드입니다

  
### 느낀점

- 요번 과제를 하면서 UI이벤트를 활용하여 게임 제작을 많이 하였는데 확실히 전에 하던 C#문법들을 활용하니 더욱 쉽게 문제 해결들이 잘된거 같습니다 역시 기초가 탄탄해야해!!