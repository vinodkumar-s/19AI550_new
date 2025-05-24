# Ex.No:10 FSM-Based Enemy AI in a 2D Platformer using Unity.                                                                     
### REGISTER NUMBER : 212222240116
### AIM: 
To develop a game flappy bird in Unity 
### Algorithm:

1. Start in Idle state
2. If idleTimer > X seconds → Transition to Patrol
3. In Patrol:
    - Move between predefined points
    - If player detected → Transition to Chase
4. In Chase:
    - Move towards player
    - If in attack range → Transition to Attack
    - If player out of range → Return to Patrol
5. In Attack:
    - Play attack animation
    - Deal damage to player
    - If player out of range → Transition to Chase
6. If health <= 0 → Transition to Dead
### Enemy State:
```
public enum EnemyState 
{
    Idle,
    Patrol,
    Chase,
    Attack,
    Dead
}
```
### Program:
Csharp
```
using UnityEngine;

public class EnemyAI : MonoBehaviour {
    public EnemyState currentState = EnemyState.Idle;

    public Transform[] patrolPoints;
    public Transform player;
    public float speed = 2f;
    public float chaseRange = 5f;
    public float attackRange = 1.5f;
    public int health = 100;

    private int currentPoint = 0;
    private float idleTimer = 0f;
    private float idleWaitTime = 2f;

    void Update() {
        switch (currentState) {
            case EnemyState.Idle:
                Idle();
                break;
            case EnemyState.Patrol:
                Patrol();
                break;
            case EnemyState.Chase:
                Chase();
                break;
            case EnemyState.Attack:
                Attack();
                break;
            case EnemyState.Dead:
                Dead();
                break;
        }

        if (health <= 0)
            currentState = EnemyState.Dead;
    }

    void Idle() {
        idleTimer += Time.deltaTime;
        if (idleTimer >= idleWaitTime) {
            idleTimer = 0;
            currentState = EnemyState.Patrol;
        }
    }

    void Patrol() {
        Transform targetPoint = patrolPoints[currentPoint];
        MoveTo(targetPoint.position);

        if (Vector2.Distance(transform.position, targetPoint.position) < 0.2f) {
            currentPoint = (currentPoint + 1) % patrolPoints.Length;
        }

        if (Vector2.Distance(transform.position, player.position) <= chaseRange) {
            currentState = EnemyState.Chase;
        }
    }

    void Chase() {
        MoveTo(player.position);

        if (Vector2.Distance(transform.position, player.position) <= attackRange) {
            currentState = EnemyState.Attack;
        }
        else if (Vector2.Distance(transform.position, player.position) > chaseRange) {
            currentState = EnemyState.Patrol;
        }
    }

    void Attack() {
        // Play animation
        Debug.Log("Enemy Attacks!");

        if (Vector2.Distance(transform.position, player.position) > attackRange) {
            currentState = EnemyState.Chase;
        }
    }

    void Dead() {
        Debug.Log("Enemy Dead!");
        Destroy(gameObject);
    }

    void MoveTo(Vector2 target) {
        transform.position = Vector2.MoveTowards(transform.position, target, speed * Time.deltaTime);
    }
}

```
### Output:


![Screenshot 2025-05-23 203417](https://github.com/user-attachments/assets/8b299d8b-46ac-4cf5-8ea3-b05aa33ce1ed)

### Result:
Thus the game was developed using Unity and it is successfully executed.
