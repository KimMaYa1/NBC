# 내일배움캠프 17일차 TIL | 유니티 입문 - 팀과제(내 꿈이 현실의 버그에 침식당하기 시작해서 위험해)

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

## 팀과제 링크
<htr>https://github.com/phw97123/B10_DreamsComeTrue

# 유니티 팀과제

## 오늘 구현하고 추가된 메서드

### 플레이어를 죽이는 오브젝트

  ```
    public class PlayerKillObjectMove : MonoBehaviour
{
    private bool _isSee = false;
    private bool _isRight = true;
    private int _spead;
    private float _x;
    private float _time = 0;
    float _sleepTime = 5;
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        if (_isSee)
        {
            Move();
        }
        else
        {
            if (_isRight)
            {
                int dir = 0;
                while (dir == 0)
                {
                    dir = Random.RandomRange(-1, 2);
                }
                _spead = 4 * dir;
                _x = -3.45f * dir;
                _isRight = false;
            }
            transform.position = new Vector3(_x, -4f, 1);
            _time += Time.deltaTime;
            if (_sleepTime < _time)
            {
                _isSee = true;
                _time = 0;
            }
        }
    }

    private void Move()
    {
        GetComponent<Rigidbody2D>().velocity = new Vector2(_spead, 0);
        if (transform.position.x < -3.5 || transform.position.x > 3.5)
        {
            _isSee = false;
            _isRight = true;
            _sleepTime = Random.RandomRange(1.5f, 3f); //1.5 ~ 3 초 사이
        }
    }
}
  ```
- 해당 메서드에서 플레이어를 죽이는 오브젝트가 특정시간마다 재위치하면서 계속 반대편으로 향하게 만들었습니다. 처음에는 Thread.Sleep으로 진행하려했지만 유티니 자체가 멈추는 것을 확인해서 Time.deltaTime을 이용하였습니다.

### 플레이어 뒤집기

  ```
  if (moveInput.x < 0)
{
    transform.Find("MainSprite").gameObject.GetComponent<SpriteRenderer>().flipX = true;
}
else if (moveInput.x > 0)
{
    transform.Find("MainSprite").gameObject.GetComponent<SpriteRenderer>().flipX = false;
}
  ```
- 해당 메서드는 입력받은 키에 따라 x값이 -1이거나 1로 입력이 되기때문에 스프라이트 랜더러에 있는 flipX로 뒤집기를 하였습니다.

### 플레이어의 각 아이템별 상호작용 & 스턴

  ```
void Update()
{
    if (_isStun)
    {
        _time += Time.deltaTime;
        if(_time > StunTime)
        {
            OutStun();
            _time = 0;
        }
    }
}

void OnTriggerEnter2D(Collider2D other)
{
    switch (other.gameObject.tag)
{
    case "BadBug":
        GetComponent<PlayerInput>().enabled = false;
        _isStun = true;
        break;
    case "FixBug":
        Score++;
        break;
    case "Battery":
        break;
    case "ChatGpt":
        break;
    case "CPU":
        break;
    case "Insecticide":
        break;
    case "NullImage":
        break;
    case "KillObject":
        IsDead = true;
        break;
}
    other.gameObject.SetActive(false);
}

private void OutStun()
{
    GetComponent<PlayerInput>().enabled = true;
    _isStun = false;
}
  ```
- 저번에 개발하였던 플레이어 컨트롤러 메서드에 각 태그에 맞게 상호작용을 진행하였고 스턴의 중복은 타임은 계속해서 증가해서 중복되지 않습니다
- 스턴 자체는 PlayerInput을 비활성화 시키는것으로 제어하였습니다.

### 향후 개발

- 오늘 점프기능을 구현하는데 PlayerMovement 스크립트에서 업데이트 부분에 계속해서 벨로시티를 정해줘서 점프가 상쇄되어 점프가 안되는것을 확인하여 내일 고쳐볼예정입니다.