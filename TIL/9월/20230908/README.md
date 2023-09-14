# 내일배움캠프 16일차 TIL | 유니티 입문 - 팀과제(내 꿈이 현실의 버그에 침식당하기 시작해서 위험해)

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

## 팀과제 링크
<htr>https://github.com/phw97123/B10_DreamsComeTrue

# 유니티 팀과제

## 오늘 구현하고 추가된 메서드

### 프리팹 추락

  ```
public class ObjectsFall : MonoBehaviour
{
    private float speed;
    private Rigidbody2D _rigidbody;
    private float x;
    private float y;

    private void Awake()
    {
        speed = Random.Range(4, 10);
        _rigidbody = GetComponent<Rigidbody2D>();
    }

    private void Start()
    {
        x = transform.position.x;
        y = transform.position.y;
    }

    private void FixedUpdate()
    {
        ApplyFall();
        if(transform.position.y < -5.5)
        {
            transform.position = new Vector3(x, y, 0);
            gameObject.SetActive(false);
        }
    }

    private void ApplyFall()
    {
        Vector2 direction = new Vector2(0,-1) * speed;
        _rigidbody.velocity = direction;
    }
}
  ```

  - 밑에서 있는 오브젝트 풀링을 사용하기 위해 처음 프리팹 생성시 위치를 저장하고 일정 Y좌표를 넘어가면 (나중에 지면이 생기면 충돌처리) 처음 소환자리로 돌아감과 동시에 비활성화 합니다

### 프리팹 생성 & 오브젝트 풀링

  ```
    public class SpawnPrefabs : MonoBehaviour
    {
    public GameObject[] Prefabs;        //0 배드 버그 1 픽스 버그 2 회복
    public GameObject[] PullObject;     //풀링 오브젝트
    private float randomX = 2.7f;
    private float Y = 5.2f;
    private float time = 0;
    private int _spawnNum = 3;
    private int _count = 0;
    public int pivot = 0;

    void Start()
    {
        PullObject = new GameObject[100];
        for(int i = 0; i < PullObject.Length; i++)
        {
            int index = Random.Range(0, 10);
            if (index > 2)
            {
                index = 0;
            }
            else if (index > 0)
            {
                index = 1;
            }
            else
            {
                index = 2;
            }
            Vector3 spawnPos = new Vector3(Random.Range(-randomX, randomX), Y, 1);
            GameObject gameObject = Instantiate(Prefabs[index], spawnPos, Prefabs[index].transform.rotation);
            PullObject[i] = gameObject;
            gameObject.SetActive(false);
        }
    }

    // Update is called once per frame
    void Update()
    {
        time += Time.deltaTime;
        if (time > 0.5f)
        {
            SpawnPrefab(_spawnNum);
        }
    }

    private void SpawnPrefab(int spawnNum)
    {
        for(int i = 0; i < spawnNum; i++)
        {
            PullObject[pivot++].SetActive(true);
            if (pivot == 100)
            {
                pivot = 0;
            }
        }
        _count++;
        if (_count > 30)
        {
            _count = 0;
            spawnNum++;
        }
        time = 0;
    }
    }
  ```


- 처음에 오브젝트 풀링이 아닌 계속 생성되는 생성과 비활성화를 시켰는데 게임을 오래하였을때 프리팹의 생성과 비활성화를 반복하면 성능의 떨어짐이 발생할수있다는 지적을 받아 오브젝트 풀링을 사용하여 프리팹을 생성하였습니다

### 향후 개발

- 15초 주기로 소환되는 개수를 늘리고 있는데 속도를 더해서 난이도를 높일 생각입니다
