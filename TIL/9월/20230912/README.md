# 내일배움캠프 18일차 TIL | 오브젝트 점프와 이동의 상쇄와 해결

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

## 팀과제 링크
<htr>https://github.com/phw97123/B10_DreamsComeTrue

# 오늘 구현하고 추가된 메서드 & 알게된 메서드

## 1. 플레이어 오브젝트 점프

### 문제 (버그가 나는 코드)
  ```
    private void FixedUpdate()
    {   
        ApplyMovement(_movementDirection);
    }
    private void Jump()
    {
        if (isGrounded) // 땅에 닿아 있다면 
        {
            _rigidbody.AddForce(Vector2.up * JumpForce, ForceMode2D.Impulse); // 위쪽 방향 * 점프할 힘, 짧고 강한 충격으로 힘을 가함
            isGrounded = false; //땅에 닿아있지 않음
            _anim.SetBool("IsJump", true); 
        }
    }
    private void OnCollisionEnter2D(Collision2D collision)
    {
        if(collision.gameObject.CompareTag("Ground"))
        {
            isGrounded = true;
            _anim.SetBool("IsJump", false);
        }
    }
    private void ApplyMovement(Vector2 direction)
    {
        direction = direction * 3 * Speed;
        _rigidbody.velocity = direction;
    }
  ```
- 처음에 점프를 하였을때 점프자체는 구동하지만 점프가 잘작동하지않아 주변동료분들에게 여쭤보니 업데이트에서 현재 리지드바디에 가속도를 지정해주어서 점프가 상쇄된다고 합니다

### 해결방안
  ```
    private void ApplyMovement(Vector2 direction)
    {
        _rigidbody.velocity = new Vector2(direction.x *3 *Speed, _rigidbody.velocity.y); 
    }
  ```
- 가속도를 지정해줄때 x값을 지정하고 점프자체의 가속도는 본인의 가속도를 가져오게하니 잘작동하였습니다.

## 향후 개발

- 팀 프로젝트의 마무리 단계를 진행할 예정으로 애니메이션을 몇개 추가할예정이며 발표자료를 만들예정입니다.
- 오늘 싱글톤에 대해서 살짝 배웠는데 내일이나 모래 좀더 깊이 파고들예정입니다.