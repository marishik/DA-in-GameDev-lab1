# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнила:
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
Познакомиться с программными средствами для организции передачи данных между инструментами google, Python и Unity

## Задание 1
Была реализована совместная работа и передача данных в связке Python - Google-Sheets – Unity. 
В облачном сервисе google console я подключила API для работы с google sheets и google drive.
Реализована запись данных из скрипта на python в google-таблицу. Данные описывают изменение темпа инфляции на протяжении 11 отсчётных периодов, с учётом стоимости игрового объекта в каждый период.
Создан новый проект на Unity, который получает данные из google-таблицы, в которую были записаны данные в предыдущем пункте.
Написан функционал на Unity, в котором будет воспризводиться аудио-файл в зависимости от значения данных из таблицы.

Код Python:
```
import gspread
import numpy as np

gc = gspread.service_account(filename = 'unityda-lr-eeb54da1f951.json')
sh = gc.open('UnitySheets')
price = np.random.randint(2000, 10000, 11)
mon = list(range(1, 11))
i = 0
while i <= len(mon):
    i +=1
    if i ==0:
        continue
    else:
        tempInf = ((price[i-1] - price[i-2]) / price[-2]) * 100
        tempInf = str(tempInf)
        tempInf = tempInf.replace('.', ',')
        sh.sheet1.update(('A' + str(i)), str(i))
        sh.sheet1.update(('B' + str(i)), str(price[i-1]))
        sh.sheet1.update(('C' + str(i)), tempInf)
        print(tempInf)
```
![изображение](https://user-images.githubusercontent.com/114138439/194706203-65f6a9f3-45d5-44bc-b640-387b6f10867b.png)


Google Sheets:

![изображение](https://user-images.githubusercontent.com/114138439/194706215-e9521947-43da-48b7-8999-9e48a935a7ec.png)


Значения в выводе PyCharm и в записи Google Sheets совпадают.

Проект в Unity и начало работы в VS Code:

![изображение](https://user-images.githubusercontent.com/114138439/194706227-345ebfde-a788-40d7-ba3f-dc7628131313.png)
![изображение](https://user-images.githubusercontent.com/114138439/194706233-d0c82a91-3cbf-4e22-a71e-074e7b289a60.png)

Конечный код в VS Code:
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;

public class NewBehaviourScript : MonoBehaviour
{
    public AudioClip goodSpeak;
    public AudioClip normalSpeak;
    public AudioClip badSpeak;
    private AudioSource selectAudio;
    private Dictionary<string,float> dataSet = new Dictionary<string, float>();
    private bool statusStart = false;
    private int i = 1;

    // Start is called before the first frame update
    void Start()
    {
        StartCoroutine(GoogleSheets());
    }

    // Update is called once per frame
    void Update()
    {
        if (dataSet["Mon_" + i.ToString()] <= 10 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] > 10 & dataSet["Mon_" + i.ToString()] < 100 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] >= 100 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
    }

    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1LhtdcRYkAWLS-OYAq4ULV14Bv4jLfk-EO2jSJPgyjkE/values/Лист1?key=AIzaSyAPeCHowkyY4u5F0oCHxqSL2zgJKSOaDfI");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[2]));
        }
    }

    IEnumerator PlaySelectAudioGood()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = goodSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioNormal()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = normalSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = badSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        i++;
    }
}
```
![изображение](https://user-images.githubusercontent.com/114138439/194693743-387e0bd3-e812-477d-9358-520fdbd2fad1.png)
![изображение](https://user-images.githubusercontent.com/114138439/194693753-12ac1d2b-ac37-4b19-a5b6-c3d9a9ea9e6c.png)


## Задание 2
### Реализовать запись в Google-таблицу набора данных, полученных с помощью линейной регрессии из лабораторной работы № 1. 

Была проделана работа по аналогии с первым заданием: та же работа с Google-Console, создание Google-таблицы, написание определенного кода для вывода в таблицу значений. 
PyCharm с кодом и полученными данными:

![изображение](https://user-images.githubusercontent.com/114138439/194702938-af747164-fe92-4319-8413-b4db154fbcb0.png)

Данные, записанные в таблицу совпадают с полученными данными в PyCharm:
![изображение](https://user-images.githubusercontent.com/114138439/194703048-45620a1b-e275-47c9-8910-bfa0ae1711d4.png)

Конечный код python:
```
import gspread
import numpy as np
import matplotlib.pyplot as plt
gc = gspread.service_account(filename = 'unityda-lr-727e43d247c3.json')
sh = gc.open('PythonFromLab1')

x = [3, 21, 22, 34, 54, 34, 55, 67, 89, 99]
x = np.array(x)

y = [2, 22, 24, 65, 79, 82, 55, 130, 150, 199]
y = np.array(y)

plt.scatter(x, y)


def model(a, b, x):
    return a * x + b


def loss_function(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    return (0.5 / num) * (np.square(prediction - y)).sum()


def optimize(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    da = (1.0 / num) * ((prediction - y) * x).sum()
    db = (1.0 / num) * ((prediction - y).sum())
    a = a - Lr * da
    b = b - Lr * db
    return a, b


def iterate(a, b, x, y, times):
    for i in range(times):
        a, b = optimize(a, b, x, y)
    return a, b


a = np.random.rand(1)
b = np.random.rand(1)
Lr = 0.00001
prev_loss = 0

for i in range(1,11):
    a, b = iterate(a, b, x, y, i)
    prediction = model(a, b, x)
    loss = loss_function(a, b, x, y)

    temp_loss = loss_function(a, b, x, y)
    l_difference = abs(temp_loss - prev_loss)
    prev_loss = temp_loss

    print(i, temp_loss, l_difference)
    sh.sheet1.update(('A' + str(i)), str(i))
    sh.sheet1.update(('B' + str(i)), str(temp_loss))
    sh.sheet1.update(('C' + str(i)), str(l_difference))
```

## Задание 3
### Самостоятельно разработать сценарий воспроизведения звукового сопровождения в Unity в зависимости от изменения считанных данных в задании 2.

Был разработан сценарий, при котором при значении меньше единицы воспроизводится звук Horosho, при значении от 1 до 1000 - звук Sredne, а более 1000 соответственно звук Ploho.
C# скрипт в unity:
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;

public class NewBehaviourScript : MonoBehaviour
{
    public AudioClip goodSpeak;
    public AudioClip normalSpeak;
    public AudioClip badSpeak;
    private AudioSource selectAudio;
    private Dictionary<string,float> dataSet = new Dictionary<string, float>();
    private bool statusStart = false;
    private int i = 1;

    // Start is called before the first frame update
    void Start()
    {
        StartCoroutine(GoogleSheets());
    }

    // Update is called once per frame
    void Update()
    {
        if (dataSet["Mon_" + i.ToString()] <= 1 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] > 1 & dataSet["Mon_" + i.ToString()] <= 1000 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] > 1000 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
    }

    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1LhtdcRYkAWLS-OYAq4ULV14Bv4jLfk-EO2jSJPgyjkE/values/Лист1?key=AIzaSyAPeCHowkyY4u5F0oCHxqSL2zgJKSOaDfI");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[2]));
        }
    }

    IEnumerator PlaySelectAudioGood()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = goodSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioNormal()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = normalSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = badSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        i++;
    }
}
```



## Выводы

В процессе лабораторной работы я познакомилась с программными средствами для организации передачи данных между инструментами google, Python и Unity. Научилась записывать данные в Google Sheets посредством написания python кода, попрактиковалась в этом, выполнив это сначала последовательно с помощью видео, а затем  закрепила знания, сделав самостоятельно. Был написан функционал на Unity, в котором воспроизводятся аудио-файлы в зависимости от значения данных из таблицы.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |
