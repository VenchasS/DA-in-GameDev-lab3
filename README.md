# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #3 выполнил(а):
- Безбородов Вениамин Васильевич
- РИ210940
Отметка о выполнении заданий (заполняется студентом):

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


Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.


## Цель работы
познакомиться с программными средствами для создания
системы машинного обучения и ее интеграции в Unity.

## Задание 1
### Реализовать систему машинного обучения в связке Python - Google-Sheets – Unity.
Ход работы:
Был создан пустой 3D проект на Unity, к ней был подключен ML Agent
Далее при помощи anaconda prompt били скачаны ```mlagents, torch```

В проекте были созданны сфера, плоскость и куб

Для сферы был написан C# [скрипт](https://github.com/VenchasS/DA-in-GameDev-lab3/blob/main/RollerAgent.cs)
```css
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    Rigidbody rBody;
    // Start is called before the first frame update
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public Transform Target;
    public override void OnEpisodeBegin()
    {
        if (this.transform.localPosition.y < 0)
        {
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }

        Target.localPosition = new Vector3(Random.value * 8 - 4, 0.5f, Random.value * 8 - 4);
    }
    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(Target.localPosition);
        sensor.AddObservation(this.transform.localPosition);
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }
    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        float distanceToTarget = Vector3.Distance(this.transform.localPosition, Target.localPosition);

        if (distanceToTarget < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }
        else if (this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }
}
```


## Задание 2
### Сделать подгрузку результатов 1 лабы в гугл таблицы.
 Файл 1 лабораторной работы был модифицирован и были сделанны 4 измерения на 1, 10, 100, 1000 итераций после чего данные об `A, B, LOSS` были внесенны в таблицу
 
Python [file](https://github.com/VenchasS/DA-in-GameDev-lab2/blob/main/task2.py)

Резульататы работы:

![Screenshot_4](https://user-images.githubusercontent.com/49115035/194925157-2758eab1-ed23-47a2-b15f-f6ba3a89e4fd.png)
![Screenshot_5](https://user-images.githubusercontent.com/49115035/194925198-7abfe5fa-0706-406c-b2f1-d537e9a0e5ea.png)



## Задание 3
### 
Был переписан скрипт из 1 задания для подгрузки данных об `A, B, LOSS` из гугл таблицы
После чего в зависимости от `LOSS` проигрывался звук:
`LOSS > 1500 = Badaudio` , `LOSS <= 1500 && LOSS > 500 = Noramlaudio` , `LOSS <= 500 = Goodaudio`
Результат работы скрипта:
![Screenshot_7](https://user-images.githubusercontent.com/49115035/194932829-a433dbd6-38dc-40ed-abe3-095220b5fa20.png)

ссылка на [скрипт](https://github.com/VenchasS/DA-in-GameDev-lab2/blob/main/NewBehaviourScript2.cs)
## Выводы
Я научился подгружать и загружать данные из гугл таблиц при помощи связки python , C#


