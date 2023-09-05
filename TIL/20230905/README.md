# 내일배움캠프 15일차 TIL | 유니티 입문 - 개인과제

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

## 오늘 만난 오류들

- 오늘 캐릭터들의 이름을 가져오기 위해 태그를 설정후 가져오려 했는데 캐릭터가 비활성화 시에는 가져오지 못하는점을 파악하지 못하다가 캐릭터들의 이름을 가져오지 못하는것을 발견 활성화를 시키고 나중에 캐릭터 이름을 가져오게 변경

- 컴포넌트의 이름을 잘못 알고 가져와 계속 못가져오다가 컴포넌트의 이름을 확인후 오류발생 원인을 알았습니다. (앞으로 컴포넌트의 이름을 잘 확인하자!)

## 유니티 개인과제

### Find메서드

  ```
public class GameObj : MonoBehaviour
{
    GameObject Obj_Canvas;            //오브젝트 선언
    GameObject Obj_Field;               //오브젝트 선언

    void Start()
    {
        Obj_Canvas = GameObject.Find("Canvas"); //오브젝트 이름이 Canvas인 것을 참조
        Obj_Field = GameObject.FindWithTag("Field"); //오브젝트 태그가 Field인 것을 참조
        gameObjects = GameObject.FindGameObjectsWithTag("Character"); // Character 태그를 가지고있는 오브젝트들을 배열로 반환 (만약 오브젝트가 한개라면 FindGameObjectWithTag를 사용해도 된다)
         a = GameObject.FindObjectOfType<FindObjecta>().transform; //FindObjecta스크립트가 들어가있는 오브젝트를 찾아줍니다
    }
}
  ```
- 특정 이름을 가졌거나 태그를 가진 오브젝트들을 찾을때 유용한 메서드입니다.
만약 자식을 찾는데 자식이 많이 없다면 GetChild 를 사용해도 괜찮을거같다.

### 만든 메서드
  ```
public void NameChange()
{
    nameInputCollection.SetActive(true);
    nameInputCollection.transform.Find("Join").gameObject.SetActive(true);
    nameInputCollection.transform.Find("NameInput").gameObject.SetActive(true);
    nameInputCollection.transform.Find("CharacterSelect").gameObject.SetActive(false);
    nameInputCollection.transform.Find("NameInput").gameObject.GetComponent<InputField>().text = "";
    _player.GetComponent<PlayerInput>().enabled = false;
}
  ```
- 캐릭터의 이름을 변경해주는 메서드인데 인풋필드에 전에 값이 계속 남아있어 공백으로 설정해주었습니다.

  ```
  using System.Collections;
  using System.Collections.Generic;
  using UnityEngine;

  public class FollowPlayer : MonoBehaviour
  {
    private GameObject _player;

    public void CatchPlayer(GameObject player)
    {
        _player = player;
    }

    void Update()
    {
        if (_player.active)
        {
            transform.position = new Vector3(_player.transform.position.x, _player.transform.position.y, transform.position.z);
        }
    }
  }
  ```
- 플레이어가 활성화되어있을때 카메라가 플레이어를 따라다니는 메서드를 만들었는데 이유가 플레이어가 활성화 되있지 않을때도 따라게해둬도 상관없지만 if문 안에 넣어두어 성능을 조금이라도 높이려고해서 사용하였습니다.
- - -
- - -
