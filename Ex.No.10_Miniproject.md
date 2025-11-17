# Ex.No: 10  Implementation of 2D Game of Flappy Bird
### DATE: 15/11/25                                                                          
### REGISTER NUMBER : 212223240068
### AIM: 
To develop a 2D Flappy-Bird style game where the player controls a character that jumps to avoid obstacles, earns points by passing through gaps, and restarts the game upon collision while tracking the best score.
### Algorithm:
```
1. Start the game with the player inactive and menu UI visible.

2. When Start is pressed, activate the player and hide the menu.

3. On each click, make the player jump by applying upward velocity.

4. Continuously spawn obstacles at random heights at fixed intervals while the player is alive.

5. When the player passes through scoring zones, increase the score.

6. When the player collides with an obstacle, deactivate player, reset score, and show menu.

7. Update best score whenever current score exceeds it.

8. Allow the user to restart by pressing Start again.
```  
### Program:
```
## ObstacleHandle.cs

using UnityEngine;

public class ObstacleHandler : MonoBehaviour
{
    [SerializeField]
    private float _moveSpeed = 10f;   
    [SerializeField]
    private float _minX = -15f; 
    void Update()
    {
        transform.position += Vector3.left * _moveSpeed * Time.deltaTime;
        if(transform.position.x <= _minX)
        {
            Destroy(gameObject);
        }
    }
}

## ObstacleSpawner.cs

using UnityEngine;

public class ObstacleSpawner : MonoBehaviour
{
    [SerializeField]
    private GameObject _obstacle;
    [SerializeField]
    private float _yOffsetMin = 10f , _yOffsetMax = -5f;
    [SerializeField]
    private PlayerController _player;
    private float _spawnFrequency = 3f;
    private float _timeTillNextSpawn = 0f;
    void Update()
    {
        if(_timeTillNextSpawn <= 0f && _player.IsAlive)
        {
            Vector3 newPosition = new Vector3(transform.position.x,Random.Range(_yOffsetMin,_yOffsetMax));
            Instantiate(_obstacle,newPosition,transform.rotation);
            _timeTillNextSpawn = _spawnFrequency;
        }
        else
        {
            _timeTillNextSpawn -= Time.deltaTime;
        }
    }
}

## PlayerController.cs

using UnityEngine;

public class PlayerController : MonoBehaviour
{
    [SerializeField]
    private float _jumpforce =10f;
    [SerializeField]
    private GameObject _menu;
    private Rigidbody2D _rigidbody;
    private int _score = 0;
    public int Score
    {
        get{
            return _score;
        }
    }
    private void Awake()
    {
        _rigidbody=GetComponent<Rigidbody2D>();
        gameObject.SetActive(false);
    }

    public bool IsAlive
    {
        get
        {
            return gameObject.activeSelf;
        }
    }
    void Update()
    {
        if(Input.GetMouseButtonDown(0))
        {
            _rigidbody.linearVelocityY=_jumpforce;
        }
    }
    private void OnCollisionEnter2D(Collision2D collision)
    {
        gameObject.SetActive(false);
        _menu.SetActive(true);
        _score = 0;
    }
    private void OnTriggerEnter2D(Collider2D collision)
    {
        _score += 1;
    }
}


## UserInterfaceHandler.cs

using TMPro;
using UnityEngine;
public class UserInterfaceHandler : MonoBehaviour
{
    [SerializeField]
    private GameObject _player;
    [SerializeField]
    private Vector3 _playerStartPosition = Vector3.zero;
    [SerializeField]
    private GameObject _menuUI;
    [SerializeField]
    private TextMeshProUGUI _scoreText,_bestScoreText;
    private PlayerController _playerController;
    private int _bestScore = 0;
    private void Start()
    {
        _playerController = _player.GetComponent<PlayerController>();
    }
    private void Update()
    {
        _scoreText.text = _playerController.Score.ToString();
        if(_playerController.Score > _bestScore)
        {
            _bestScore=_playerController.Score;
            _bestScoreText.text = _bestScore.ToString();
        }
    }

    public void OnStartPressed()
    {
        _player.SetActive(true);
        _player.transform.position = _playerStartPosition;
        _menuUI.SetActive(false);
    }
}

```
### Output:

<img width="1920" height="1080" alt="Screenshot (4)" src="https://github.com/user-attachments/assets/72c3b03a-0fe9-4928-a005-5ee8c96c880f" />

### Result:
Thus the game was developed using Unity and adopted machine learning–based agent control technology.

