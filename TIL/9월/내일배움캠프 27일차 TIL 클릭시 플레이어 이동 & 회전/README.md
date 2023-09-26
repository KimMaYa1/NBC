# 내일배움캠프 27일차 TIL |  클릭시 플레이어 이동 & 회전

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

### [팀프로젝트 경로](https://github.com/KimMaYa1/B09_LostIsland)

## 팀프로젝트 - 로스트 아일랜드

### 역할 - 플레이어 인풋 시스템, 스텟, 죽었을시 불이익 / 카메라 관리  

## 상호작용 매니저 전체 코드
<details>
<summary>코드</summary>

  ```
using TMPro;
using UnityEngine;
using UnityEngine.InputSystem;

public class InteractionManager : MonoBehaviour
{
    public float checkRate = 0.05f;
    private float lastCheckTime;
    public float maxCheckDistance;
    public LayerMask interactLayerMask;
    public LayerMask itemLayerMask;

    private GameObject curInteractGameObject;
    private Item curInteractable;

    public TextMeshProUGUI promptText;
    private Camera camera;

    void Start()
    {
        camera = Camera.main;
    }

    void Update()
    {
        if (Time.time - lastCheckTime > checkRate)
        {
            lastCheckTime = Time.time;

            Collider[] colls = Physics.OverlapSphere(transform.position, maxCheckDistance, itemLayerMask);
            float min = maxCheckDistance;
            for(int i = 0; i < colls.Length; i++)
            {
                if ((colls[i].gameObject.transform.position - transform.position).magnitude < maxCheckDistance)
                {
                    float dis = Vector3.Distance(colls[i].gameObject.transform.position, transform.position);
                    min = Mathf.Min(min, dis);
                    if (colls[i].gameObject != curInteractGameObject)
                    {
                        if (dis <= min)
                        {
                            curInteractGameObject = colls[i].gameObject;
                            curInteractable = colls[i].GetComponent<ItemPickUp>().item;
                            SetPromptText();
                        }
                    }
                }
                else
                {
                    curInteractGameObject = null;
                    curInteractable = null;
                    promptText.gameObject.SetActive(false);
                }
            }
        }
    }

    private void SetPromptText()
    {
        promptText.gameObject.SetActive(true);
        promptText.text = string.Format("<b>[F]</b> {0}", curInteractable.Interactable());
    }

    public void OnInteractInput(InputAction.CallbackContext callbackContext)
    {
        if (callbackContext.phase == InputActionPhase.Started && curInteractable != null)
        {
            //curInteractable.OnInteract();
            curInteractGameObject = null;
            curInteractable = null;
            promptText.gameObject.SetActive(false);
        }
    }
}

  ```
</details>

## 근처 아이템들을 가져와 어떤 아이템인지 띄워주는 코드

### 시도하려한것들

- 처음에는 아이템 배열들을 받아오고 각 아이템과의 거리를 계산해 거리가 짧은 순서대로 정렬 시켜 일정거리안에 있으면 가장 가까운 아이템을 띄워주려했는데 아직 맵의 크기도 정해지지 않았고 아이템이 얼마나 많을지 모르기 때문에 Physics를 사용하기로 했습니다.

### 해결방안
<details>
<summary>코드</summary>

  ```
Collider[] colls = Physics.OverlapSphere(transform.position, maxCheckDistance, itemLayerMask);
float min = maxCheckDistance;
for(int i = 0; i < colls.Length; i++)
{
    if ((colls[i].gameObject.transform.position - transform.position).magnitude < maxCheckDistance)
    {
        float dis = Vector3.Distance(colls[i].gameObject.transform.position, transform.position);
        min = Mathf.Min(min, dis);
        if (colls[i].gameObject != curInteractGameObject)
        {
            if (dis <= min)
            {
                curInteractGameObject = colls[i].gameObject;
                curInteractable = colls[i].GetComponent<ItemPickUp>().item;
                SetPromptText();
            }
        }
    }
    else
    {
        curInteractGameObject = null;
        curInteractable = null;
        promptText.gameObject.SetActive(false);
    }
}

private void SetPromptText()
{
    promptText.gameObject.SetActive(true);
    promptText.text = string.Format("<b>[F]</b> {0}", curInteractable.Interactable());
}
  ```
</details>

- 해당 메서드에서 Physics.OverlapSphere 을 사용하여 나의 포지션에서 maxCheckDistance의 반경을 가지는 원만큼을 탐색해 itemLayerMask에 할당된 레이어를 가지고있는 아이템배열을 colls에 넣어줍니다
- 배열중에 가장 가까운 아이템의 이름을 SetPromptText에서 띄워줍니다

## 이동 메서드 전체 코드
<details>
<summary>코드</summary>

  ```
using System.Collections;
using System.Collections.Generic;
using Unity.VisualScripting;
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerClickMove : MonoBehaviour
{
    private Camera camera;
    private Vector3 destination;
    private Vector3 direction;
    private PlayerController playerController;
    private Rigidbody _rigidbody;
    private bool isMove;

    private void Awake()
    {

        _rigidbody = GetComponent<Rigidbody>();
    }

    // Start is called before the first frame update
    void Start()
    {
        playerController = PlayerController.instance;
        camera = Camera.main;
    }

    private void Update()
    {
        if (isMove)
        {
            Move();
        }
        else if (_rigidbody.velocity.y == 0)
        {
            _rigidbody.velocity = Vector3.zero;
        }
        transform.rotation = Quaternion.Lerp(transform.rotation, Quaternion.LookRotation(direction), 0.25f);
    }

    private void Move()
    {
        if (Vector3.Distance(destination, transform.position) <= 0.1f)
        {
            isMove = false;
            return;
        }

        Vector3 dir = direction.normalized;
        dir *= playerController.playerStat.MoveSpeed;
        dir.y = _rigidbody.velocity.y;

        _rigidbody.velocity = dir;

        destination.y = transform.position.y;
        isMove = (transform.position - destination).magnitude > 0.05f ;
    }

    public void OnClickMoveInput(InputAction.CallbackContext context)
    {
        if (IsGrounded())
        {
            if (context.phase == InputActionPhase.Canceled)
            {
                Ray ray = camera.ScreenPointToRay(Input.mousePosition);
                RaycastHit hit;

                if (Physics.Raycast(ray, out hit, 100f))
                {
                    if (hit.collider.gameObject.layer != gameObject.layer)
                    {
                        isMove = true;
                        destination = new Vector3(hit.point.x, transform.position.y, hit.point.z);
                        direction = destination - transform.position;
                    }
                }
            }
        }
    }

    public void OnJumpInput(InputAction.CallbackContext context)
    {
        if (context.phase == InputActionPhase.Started)
        {
            if (IsGrounded())
                _rigidbody.AddForce(Vector2.up * playerController.playerStat.JumpForce, ForceMode.Impulse);
        }
    }

    private bool IsGrounded()
    {
        Ray[] rays = new Ray[4]        //앞 뒤 왼 오 에다가 ray만들어서 그라운드와 만나고있는지 확인
        {
            new Ray(transform.position + (transform.forward * 0.2f) + (Vector3.up * 0.01f), Vector3.down),
            new Ray(transform.position + (-transform.forward * 0.2f) + (Vector3.up * 0.01f), Vector3.down),
            new Ray(transform.position + (transform.right * 0.2f) + (Vector3.up * 0.01f), Vector3.down),
            new Ray(transform.position + (-transform.right * 0.2f) + (Vector3.up * 0.01f), Vector3.down),
        };
        for (int i = 0; i < rays.Length; i++)
        {
            if (Physics.Raycast(rays[i], 1f, playerController.groundLayerMask))
            {
                return true;
            }
        }

        return false;
    }
}

  ```
</details>

## 바꾼점들

- 플레이어 컨트롤러에서 관리하던 이동과 점프를 따로 빼서 스크립트를 제작 (스크립트가 너무 길어지는거 같아 작업이 힘들어져 따로 관리)
- wasd를 이용해 움직이던 플레이어를 마우스 클릭으로 이동하게 바꿧습니다.
- 플레이어가 회전할때 좀더 자연스럽도록 바꿧습니다.

## 바뀐 이동 부분과 회전
<details>
<summary>코드</summary>

  ```
private void Update()
    {
        if (isMove)
        {
            Move();
        }
        else if (_rigidbody.velocity.y == 0)
        {
            _rigidbody.velocity = Vector3.zero;
        }
        transform.rotation = Quaternion.Lerp(transform.rotation, Quaternion.LookRotation(direction), 0.25f);
    }

    private void Move()
    {
        if (Vector3.Distance(destination, transform.position) <= 0.1f)
        {
            isMove = false;
            return;
        }

        Vector3 dir = direction.normalized;
        dir *= playerController.playerStat.MoveSpeed;
        dir.y = _rigidbody.velocity.y;

        _rigidbody.velocity = dir;

        destination.y = transform.position.y;
        isMove = (transform.position - destination).magnitude > 0.05f ;
    }

    public void OnClickMoveInput(InputAction.CallbackContext context)
    {
        if (IsGrounded())
        {
            if (context.phase == InputActionPhase.Canceled)
            {
                Ray ray = camera.ScreenPointToRay(Input.mousePosition);
                RaycastHit hit;

                if (Physics.Raycast(ray, out hit, 100f))
                {
                    if (hit.collider.gameObject.layer != gameObject.layer)
                    {
                        isMove = true;
                        destination = new Vector3(hit.point.x, transform.position.y, hit.point.z);
                        direction = destination - transform.position;
                    }
                }
            }
        }
    }
  ```
</details>

  - OnClickMoveInput 메서드를 플레이어 인풋에 넣어 활용 (활성 버튼은 오른쪽 마우스 버튼)
  - 클릭한 오브젝트의 레이어가 메인 플레이어가 아니면 위치를 저장하고 방향을 계산합니다.
  - Move 메서드로 클릭한 위치로 이동을 하고 근처에서 멈추도록 설정하였습니다.
  - destination.y = transform.position.y; 로 지정 포인트를 점프로 넘어가도 isMove가 false값이 되도록 설정 하였습니다.
  - 점프 중간에 갑자기 멈추는 것이 이상할수있기에 만약 플레이어의 y벨로시티가 변하고 있다면 가속도를 유지하도록 설정하였습니다.

## 앞으로 구현하고 싶은 것들

- 아이템을 줍는것을 상호작용으로 할 예정이지만 만약 시간이 남아서 다른 작업을 할수있다면 근처에 있으면 상호작용이 아닌 아이템을 클릭하면 해당아이템에 다가가 줍도록 만들고 싶습니다.

## 오늘의 한마디

- 오늘은 정말 많은것을 바꾼거 같다 앞으로는 더 많은것을 바꾸어 나자신을 가꿧으면 좋겠다.