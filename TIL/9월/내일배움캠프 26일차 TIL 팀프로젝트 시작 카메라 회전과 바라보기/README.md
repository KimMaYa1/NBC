# 내일배움캠프 26일차 TIL |  팀프로젝트 시작 카메라 회전과 바라보기

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

### [팀프로젝트 경로](https://github.com/KimMaYa1/B09_LostIsland)

## 팀프로젝트 - 로스트 아일랜드

### 역할 - 플레이어 인풋 시스템, 스텟, 죽었을시 불이익 / 카메라 관리  

## 전체 코드

  ```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.InputSystem;
using static UnityEditor.Timeline.TimelinePlaybackControls;

public class PlayerController : MonoBehaviour
{
    [Header("Movemet")]
    public float moveSpeed;
    private Vector2 curMovementInput;
    public float jumpForce;
    public LayerMask groundLayerMask;

    [Header("Look")]
    public Transform cameraContainer;
    public Transform target;
    public float camRotSpeed;
    private float camCurYRot;

    private Vector2 lookPhase;

    [HideInInspector]
    public bool canLook = true;
    
    private Rigidbody _rigidbody;
    public float delayTime = 0;
    public bool IsAttackDelay = true;
    public static PlayerController instance;

    private void Awake()
    {
        instance = this;
        _rigidbody = GetComponent<Rigidbody>();
    }

    void Start()
    {
        Cursor.lockState = CursorLockMode.Locked;
    }

    private void FixedUpdate()
    {
        if(delayTime >= 0.55f)
        {
            IsAttackDelay = true;
            delayTime = 0;
        }
        else if (!IsAttackDelay)
        {
            delayTime += Time.fixedDeltaTime;
        }
        if (IsAttackDelay)
        {
            Move();
        }

    }

    private void LateUpdate()
    {
        if (canLook)
        {
            CameraLook();
        }
    }

    private void Move()
    {
        Vector3 dir = transform.forward * curMovementInput.y + transform.right * curMovementInput.x;
        dir *= moveSpeed;
        dir.y = _rigidbody.velocity.y;
        cameraContainer.position = transform.position;

        _rigidbody.velocity = dir;

        if (_rigidbody.velocity.x != 0 || _rigidbody.velocity.z != 0)
        {
            transform.LookAt(target);
        }
    }

    void CameraLook()
    {
        camCurYRot += lookPhase.x * camRotSpeed * Time.deltaTime;  
        cameraContainer.localEulerAngles = new Vector3(0, -camCurYRot, 0);
        //transform.eulerAngles += new Vector3(0, mouseDelta.x * lookSensitiveity, 0);
    }

    public void OnLookInput(InputAction.CallbackContext context)
    {
        if( context.phase == InputActionPhase.Performed)
        {
            lookPhase = context.ReadValue<Vector2>();
        }
        else if (context.phase == InputActionPhase.Canceled)
        {
            lookPhase = Vector2.zero;
        }
    }

    public void OnMoveInput(InputAction.CallbackContext context)
    {
        if (context.phase == InputActionPhase.Performed)
        {
            curMovementInput = context.ReadValue<Vector2>();
        }
        else if (context.phase == InputActionPhase.Canceled)
        {
            curMovementInput = Vector2.zero;
        }
    }

    public void OnJumpInput(InputAction.CallbackContext context)
    {
        if (context.phase == InputActionPhase.Started)
        {
            if (IsGrounded())
                _rigidbody.AddForce(Vector2.up * jumpForce, ForceMode.Impulse);
        }
    }
    
    private bool IsGrounded()
    {
        Ray[] rays = new Ray[4]        
        {
            new Ray(transform.position + (transform.forward * 0.2f) + (Vector3.up * 0.01f), Vector3.down),
            new Ray(transform.position + (-transform.forward * 0.2f) + (Vector3.up * 0.01f), Vector3.down),
            new Ray(transform.position + (transform.right * 0.2f) + (Vector3.up * 0.01f), Vector3.down),
            new Ray(transform.position + (-transform.right * 0.2f) + (Vector3.up * 0.01f), Vector3.down),
        };
        for (int i = 0; i < rays.Length; i++)
        {
            if (Physics.Raycast(rays[i], 1f, groundLayerMask))
            {
                return true;
            }
        }

        return false;
    }

    public void OnAttackInput(InputAction.CallbackContext context)
    {
        if (IsAttackDelay)
        {
            if (context.phase == InputActionPhase.Started)
            {
                Debug.Log("Attack");
                IsAttackDelay = false;
            }
        }
    }

    public void OnInteractionInput(InputAction.CallbackContext context)
    {
        if (context.phase == InputActionPhase.Started)
        {

        }
    }

    public void OnRunInput(InputAction.CallbackContext context)
    {
        if (context.phase == InputActionPhase.Started)
        {

        }
    }

    public void OnInventoryInput(InputAction.CallbackContext context)
    {
        if (context.phase == InputActionPhase.Started)
        {

        }
    }
}

  ```

- 하단의 3개의 메서드는 아직 구현하지 않음

## 카메라 회전과 타겟 바라보기

  ```
    private void Move()
    {
        Vector3 dir = transform.forward * curMovementInput.y + transform.right * curMovementInput.x;
        dir *= moveSpeed;
        dir.y = _rigidbody.velocity.y;
        cameraContainer.position = transform.position;

        _rigidbody.velocity = dir;

        if (_rigidbody.velocity.x != 0 || _rigidbody.velocity.z != 0)
        {
            transform.LookAt(target);
        }
    }

    void CameraLook()
    {
        camCurYRot += lookPhase.x * camRotSpeed * Time.deltaTime;  
        cameraContainer.localEulerAngles = new Vector3(0, -camCurYRot, 0);
        //transform.eulerAngles += new Vector3(0, mouseDelta.x * lookSensitiveity, 0);
    }

    public void OnLookInput(InputAction.CallbackContext context)
    {
        if( context.phase == InputActionPhase.Performed)
        {
            lookPhase = context.ReadValue<Vector2>();
        }
        else if (context.phase == InputActionPhase.Canceled)
        {
            lookPhase = Vector2.zero;
        }
    }
  ```

  - CameraLook 메서드에서는 메인 카메라를 넣어둔 빈오브젝트를 돌려서 회전을 하게했고
    OnLookInput메서드에서는 Q 와 E를 누르고 있는도중에 회전을 어느쪽으로할지 알려줍니다.
  - Move 메서드에서는 이동을 담당하지만 이동시에 카메라를 넣어둔 빈오브젝트에 타겟을 넣어둬 타겟을 바라보게합니다.

  ![Alt text](2023-09-25-20-29-27-_online-video-cutter.com_.gif)

  ## 앞으로의 구현사항
  - 플레이어의 애니메이션을 넣어서 역동적으로 만들예정
  - 타겟을바라볼때 로테이션값을 지정하는것이 아닌 자연스럽게 돌도록 할예정입니다.
  
  # 한마디
  요번 프로젝트는 처음으로 3D를 사용하게 되었는데 앞으로 무엇을 배울수있을지 정말 궁금합니다 앞으로도 더 많은 것을 배워서 만들고싶은 게임을 만들수있는 개발자가 되고 말거에욥 앞으로도 화이팅!!