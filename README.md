# Лабораторная работа #4. Доработка интерактивного приложения и его подготовка к сборке.
Отчет по лабораторной работе #4 выполнил(а):
- Данькин Сергей Викторович
- РИ-300016

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[Проект DragonPicker](https://github.com/S1GARETA/Dragon_Picker) Unity 2021.3.10f1

## Цель работы

#### Подготовить разрабатываемое интерактивное приложение к сборке и публикации.

## Задание 1

#### Используя видео-материалы практических работ 1-5 повторить реализацию приведенного ниже функционала:

1. Создание анимации объектов на сцене
2. Создание стартовой сцены и переключение между ними.
3. Доработка меню и финкционала с остановкой игры.
4. Добавление звукого сопровождения в игре.
5. Добавление персонажа и сборка сцены для публикации на web-ресурсе.

## Ход работы

#### 1. Создание анимации объектов на сцене

Продолжая работу над проектом, нужно добавить в игру стартовое меню, откуда игрок сможет запускать сам игровой процесс. Для этого нужно подготовить это меню. Переименнуем нулевую сцену в первую и создадим её копию, которая будет называться "_0Scene", это будет стартовое меню.

![img](https://github.com/S1GARETA/UnityLab4/blob/main/Demo%20files/Task1.1.jpg)

Внесём некоторые изменения в нулевую сцену. Удалим лишние объекты и скрипты с объектов, так как они не понадобятся. Так же изменим положение горы и дракона так, чтобы был более призентабельный вид.

![img](https://github.com/S1GARETA/UnityLab4/blob/main/Demo%20files/Task1.2.jpg)

Изменим так же анимацию дракона, создав новый контроллер EnemyIDLE. Добавив в него анимацию сидящего дракона.

![gif](https://github.com/S1GARETA/UnityLab4/blob/main/Demo%20files/1.1.gif)

Чтобы меню стало динамичнее, добавим на сцену облако, скачав определенный ассет из Unity Asset Story. И создадим анимацию движения этого облака влево-вправо.

![img](https://github.com/S1GARETA/UnityLab4/blob/main/Demo%20files/Task1.3.jpg)

![gif](https://github.com/S1GARETA/UnityLab4/blob/main/Demo%20files/1.2.gif)

#### 2. Создание стартовой сцены и переключение между ними.

Добавляем на стартовую сцену название игры и три кнопки: play, option и quit. Также создаём пустой объект MainMenu и создаём [скрипт](https://github.com/S1GARETA/Dragon_Picker/blob/main/DragonPicker/Assets/_Scripts/MainMenu.cs) с таким же названием. В скрипте прописываем два метода - PlayGame, который отвечает за переключение на игровую сцену, и QuitGame, отвечающий за выход из игры.

```cs
using UnityEngine;
using UnityEngine.SceneManagement;

public class MainMenu : MonoBehaviour
{
    public void PlayGame()
    {
        SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex + 1);
    }

    public void QuitGame()
    {
        Application.Quit();
    }
}
```

![gif](https://github.com/S1GARETA/UnityLab4/blob/main/Demo%20files/1.3.gif)

#### 3. Доработка меню и финкционала с остановкой игры.

Добавляем панель настроек и пеключение на неё с помощью SetActive.
На геймплейную сцену добавляем надпись паузы, а также создаём [скрипт Pause](https://github.com/S1GARETA/Dragon_Picker/blob/main/DragonPicker/Assets/_Scripts/Pause.cs), благодаря которому можно ставить игру на паузу (Space) и выходить в меню (Escape).

```cs
using UnityEngine;
using UnityEngine.SceneManagement;

public class Pause : MonoBehaviour
{
    private bool paused = false;
    public GameObject panel;
    void Update()
    {
        if(Input.GetKeyDown(KeyCode.Space))
        {
            if(!paused)
            {
                Time.timeScale = 0;
                paused = true;
                panel.SetActive(true);
            }
            else
            {
                Time.timeScale = 1;
                paused = false;
                panel.SetActive(false);
            }
        }

        if(Input.GetKeyDown(KeyCode.Escape))
        {
            SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex - 1);
        }
    }
}
```

![gif](https://github.com/S1GARETA/UnityLab4/blob/main/Demo%20files/1.4.gif)

#### 4. Добавление звукого сопровождения в игре.

Из Unity Asset Story добавляем в проект звуковые эффекты взрывов и фоновые треки. К объектам добавляем компонент AudioSource и в них ссылки на AudioClip.
В скрипты [DragonEgg](https://github.com/S1GARETA/Dragon_Picker/blob/main/DragonPicker/Assets/_Scripts/DragonEgg.cs) и [EnergyShield](https://github.com/S1GARETA/Dragon_Picker/blob/main/DragonPicker/Assets/_Scripts/EnergyShield.cs) добавляем несколько строк кода, чтобы звуки взрывов и ловли воспроизводились в нужные моменты.

```cs
audioSource = GetComponent<AudioSource>();
audioSource.Play();
```

https://user-images.githubusercontent.com/96253861/199208388-fe3e945c-1847-46d9-a099-c2ce36af13d4.mp4

#### 5. Добавление персонажа и сборка сцены для публикации на web-ресурсе.

С сайта mixamo.com скачиваем и импортируем модель персонажа с его анимацией. На сцену добавляем префаб мага и настраиваем его анимацию. Для антуража добавляем на сцену источник света.
Создадим сборку на WebGL.

![gif](https://github.com/S1GARETA/UnityLab4/blob/main/Demo%20files/1.6.gif)

## Задание 2
#### Привести описание того, как происходит сборка проекта под другие платформы. Какие могут быть особенности?

### Ход работы:

Чтобы создать сборку проекта под другую платформу, нужно зайти в Build Settings и выбрать интересующую нас платформу.

Из особенностей можно выделить "Платформенно зависимую компиляцию". Она позволяет разделить скрипты для компиляции и выполнения части кода исключительно для одной из поддерживаемых платформ. Для этого нужно написать скрипт, который будет определять платформу.

```cs
using UnityEngine;
using System.Collections;

public class PlatformDefines : MonoBehaviour {
  void Start () {

    #if UNITY_EDITOR
      Debug.Log("Unity Editor");
    #endif
    
    #if UNITY_IOS
      Debug.Log("Iphone");
    #endif

    #if UNITY_STANDALONE_OSX
    Debug.Log("Stand Alone OSX");
    #endif

    #if UNITY_STANDALONE_WIN
      Debug.Log("Stand Alone Windows");
    #endif

  }          
}
```

Также, можно выделить то, что при выполнении сборки для консолей PlayStation, Xbox и Nintendo, нужно быть зарегистрированным разработчиком консолей.

## Задание 3
#### Добавить в меню Option возможность изменения громкости (от 0 до 100%) фоновой музыки в игре.

### Ход работы:

1. Добавить в меню настроек UI объект - слайдер.
2. Написать новый скрипт AudioController, с помощью которого будет изменяться громкость звуков в игре, а так же запоминаться значение громкости.

```cs
using UnityEngine;
using UnityEngine.UI;

public class AudioController : MonoBehaviour
{
    [SerializeField] private Slider volumeSlider;

    void Start() 
    {
        if(!PlayerPrefs.HasKey("soundVolume"))
        {
            PlayerPrefs.SetFloat("soundVolume", 1);
            Load();
        }
        else
        {
            Load();
        }
    }
    public void ChangeVolume()
    {
        AudioListener.volume = volumeSlider.value;
        Save();
    }
    private void Load()
    {
        volumeSlider.value = PlayerPrefs.GetFloat("soundVolume");
    }

    private void Save()
    {
        PlayerPrefs.SetFloat("soundVolume", volumeSlider.value);
    }
}
```

видео



## Выводы
