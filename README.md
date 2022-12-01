# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #5 выполнила:
- Алексеева Марина Геннадьевна
- РИ210931
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

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

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
- ✨Magic ✨

## Цель работы


## Задание 1
### Изменитm параметры файла .yaml-агента и определить какие параметры и как влияют на обучение модели.

Ход работы:
1. Открыла предоставленный мне проект Unity 

![изображение](https://user-images.githubusercontent.com/114138439/205127608-6cef950c-3d7b-40bd-8164-c1436df74a0b.png)


2. Открыла и ознакомилась со скрипт-файлом Move.cs, который описывает поведение при движении Агента. А также его вознаграждение и обучение:
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class Move : Agent
{
    [SerializeField] private GameObject goldMine;
    [SerializeField] private GameObject village;
    private float speedMove;
    private float timeMining;
    private float month;
    private bool checkMiningStart = false;
    private bool checkMiningFinish = false;
    private bool checkStartMonth = false;
    private bool setSensor = true;
    private float amountGold;
    private float pickaxeСost;
    private float profitPercentage;
    private float[] pricesMonth = new float[2];
    private float priceMonth;
    private float tempInf;

    // Start is called before the first frame update
    public override void OnEpisodeBegin()
    {
        // If the Agent fell, zero its momentum
        if (this.transform.localPosition != village.transform.localPosition)
        {
            this.transform.localPosition = village.transform.localPosition;
        }
        checkMiningStart = false;
        checkMiningFinish = false;
        checkStartMonth = false;
        setSensor = true;
        priceMonth = 0.0f;
        pricesMonth[0] = 0.0f;
        pricesMonth[1] = 0.0f;
        tempInf = 0.0f;
        month = 1;
    }
    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(speedMove);
        sensor.AddObservation(timeMining);
        sensor.AddObservation(amountGold);
        sensor.AddObservation(pickaxeСost);
        sensor.AddObservation(profitPercentage);
    }

    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        if (month < 3 || setSensor == true)
        {
            speedMove = Mathf.Clamp(actionBuffers.ContinuousActions[0], 1f, 10f);
            Debug.Log("SpeedMove: " + speedMove);
            timeMining = Mathf.Clamp(actionBuffers.ContinuousActions[1], 1f, 10f);
            Debug.Log("timeMining: " + timeMining);
            setSensor = false;
            if (checkStartMonth == false)
            {
                Debug.Log("Start Coroutine StartMonth");
                StartCoroutine(StartMonth());
            }

            if (transform.position != goldMine.transform.position & checkMiningFinish == false)
            {
                transform.position = Vector3.MoveTowards(transform.position, goldMine.transform.position, Time.deltaTime * speedMove);
            }

            if (transform.position == goldMine.transform.position & checkMiningStart == false)
            {
                Debug.Log("Start Coroutine StartGoldMine");
                StartCoroutine(StartGoldMine());
            }

            if (transform.position != village.transform.position & checkMiningFinish == true)
            {
                transform.position = Vector3.MoveTowards(transform.position, village.transform.position, Time.deltaTime * speedMove);
            }

            if (transform.position == village.transform.position & checkMiningStart == true)
            {
                checkMiningFinish = false;
                checkMiningStart = false;
                setSensor = true;
                amountGold = Mathf.Clamp(actionBuffers.ContinuousActions[2], 1f, 10f);
                Debug.Log("amountGold: " + amountGold);
                pickaxeСost = Mathf.Clamp(actionBuffers.ContinuousActions[3], 100f, 1000f);
                Debug.Log("pickaxeСost: " + pickaxeСost);
                profitPercentage = Mathf.Clamp(actionBuffers.ContinuousActions[4], 0.1f, 0.5f);
                Debug.Log("profitPercentage: " + profitPercentage);

                if (month != 2)
                {
                    priceMonth = pricesMonth[0] + ((pickaxeСost + pickaxeСost * profitPercentage) / amountGold);
                    pricesMonth[0] = priceMonth;
                    Debug.Log("priceMonth: " + priceMonth);
                }
                if (month == 2)
                {
                    priceMonth = pricesMonth[1] + ((pickaxeСost + pickaxeСost * profitPercentage) / amountGold);
                    pricesMonth[1] = priceMonth;
                    Debug.Log("priceMonth: " + priceMonth);
                }

            }
        }
        else
        {
            tempInf = ((pricesMonth[1] - pricesMonth[0]) / pricesMonth[0]) * 100;
            if (tempInf <= 6f)
            {
                SetReward(1.0f);
                Debug.Log("True");
                Debug.Log("tempInf: " + tempInf);
                EndEpisode();
            }
            else
            {
                SetReward(-1.0f);
                Debug.Log("False");
                Debug.Log("tempInf: " + tempInf);
                EndEpisode();
            }
        }
    }

    IEnumerator StartGoldMine()
    {
        checkMiningStart = true;
        yield return new WaitForSeconds(timeMining);
        Debug.Log("Mining Finish");
        checkMiningFinish = true;
    }

    IEnumerator StartMonth()
    {
        checkStartMonth = true;
        yield return new WaitForSeconds(60);
        checkStartMonth = false;
        month++;

    }
}
```
3. Запустила Anaconda Promt и написала в него ряд команд:
```
conda create -n MLAgents python=3.6
conda activate MLAgents
```

```
pip install mlagents==0.28.0
```

```
pip install torch~=1.7.1 -f https://download.pytorch.org/whl/torch_stable.html
```

![изображение](https://user-images.githubusercontent.com/114138439/205098858-0350a796-68f9-415b-b706-ef33a639dab1.png)
![изображение](https://user-images.githubusercontent.com/114138439/205099011-97e3a4dc-fcbc-4fc0-8196-e00a122caa74.png)
![изображение](https://user-images.githubusercontent.com/114138439/205099098-0583c593-0bb2-4393-a30d-308f05d8ac0c.png)
![изображение](https://user-images.githubusercontent.com/114138439/205099231-7c31a2df-2b7d-43fd-a167-c1c1eed71f2c.png)




![изображение](https://user-images.githubusercontent.com/114138439/205104221-26cabe39-23a9-4965-816c-c683165b2389.png)
![изображение](https://user-images.githubusercontent.com/114138439/205108916-6a1c6d10-84d4-4bd5-9bcc-21cf70b1659c.png)
![изображение](https://user-images.githubusercontent.com/114138439/205126502-51011dac-8b5e-45bb-b97e-beee7977c4c3.png)


![изображение](https://user-images.githubusercontent.com/114138439/205110687-54b57ddf-7b4a-4053-8e12-785a502be155.png)

![изображение](https://user-images.githubusercontent.com/114138439/205110575-619f54b5-299f-4d15-b84e-126df0c3b7c3.png)

![изображение](https://user-images.githubusercontent.com/114138439/205112218-247caae5-c310-467d-83b4-c6a93e9a7396.png)

Начальные значения 
![изображение](https://user-images.githubusercontent.com/114138439/205114699-f05323a0-92ff-49f3-b7a7-a5e9c941c00d.png)
![изображение](https://user-images.githubusercontent.com/114138439/205112497-13903a8c-1807-40eb-b3fc-74aa17e288bd.png)

![изображение](https://user-images.githubusercontent.com/114138439/205125113-b3124b74-c554-41fb-af38-7c96cae1865f.png)



![изображение](https://user-images.githubusercontent.com/114138439/205115471-9c380c8b-2f02-4ef7-bcd6-13b62d867564.png)

измененные значения и обучение
![изображение](https://user-images.githubusercontent.com/114138439/205117231-43d832ba-22c5-45cc-99fe-a36b696d132b.png)

![изображение](https://user-images.githubusercontent.com/114138439/205116248-d6233bba-c67c-44b8-a570-8c104bcd14af.png)

![изображение](https://user-images.githubusercontent.com/114138439/205125824-e158955d-86b6-49b8-a08e-5263d6ec6ca7.png)






```

```

## Задание 2
###



## Задание 3

### 




## Выводы

В процессе лабораторной работы я познакомилась с основными операторами языка Python на примере реализации линейной регрессии, научилась выводить на консоль сообщения в Unity, ознакомилась с процессом работы в среде Google Colab.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |
