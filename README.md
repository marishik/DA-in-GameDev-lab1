# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 выполнила:
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
Познакомиться с работой перцептрона на практике при помощи Unity. Обучить перцептрон логическим операциям.

## Задание 1
### В проекте Unity реализовать перцептрон, который умеет производить вычисления: OR, AND, NAND, XOR и дать комметрарии о корректности работы.
 
 Ход работы:
1. Создала пустой объект в Unity
2. Взяла предоставленный мне скрипт-файл с описанием работы перцептрона и "навесила" его на свой созданный пустой объект

![изображение](https://user-images.githubusercontent.com/114138439/204861848-5ddc296a-2935-4ed2-9c02-1f93db82fa27.png)

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
	public double[] input;
	public double output;
}

public class Perceptron : MonoBehaviour {

	public TrainingSet[] ts;
	double[] weights = {0,0};
	double bias = 0;
	double totalError = 0;

	double DotProductBias(double[] v1, double[] v2) 
	{
		if (v1 == null || v2 == null)
			return -1;
	 
		if (v1.Length != v2.Length)
			return -1;
	 
		double d = 0;
		for (int x = 0; x < v1.Length; x++)
		{
			d += v1[x] * v2[x];
		}

		d += bias;
	 
		return d;
	}

	double CalcOutput(int i)
	{
		double dp = DotProductBias(weights,ts[i].input);
		if(dp > 0) return(1);
		return (0);
	}

	void InitialiseWeights()
	{
		for(int i = 0; i < weights.Length; i++)
		{
			weights[i] = Random.Range(-1.0f,1.0f);
		}
		bias = Random.Range(-1.0f,1.0f);
	}

	void UpdateWeights(int j)
	{
		double error = ts[j].output - CalcOutput(j);
		totalError += Mathf.Abs((float)error);
		for(int i = 0; i < weights.Length; i++)
		{			
			weights[i] = weights[i] + error*ts[j].input[i]; 
		}
		bias += error;
	}

	double CalcOutput(double i1, double i2)
	{
		double[] inp = new double[] {i1, i2};
		double dp = DotProductBias(weights,inp);
		if(dp > 0) return(1);
		return (0);
	}

	void Train(int epochs)
	{
		InitialiseWeights();
		
		for(int e = 0; e < epochs; e++)
		{
			totalError = 0;
			for(int t = 0; t < ts.Length; t++)
			{
				UpdateWeights(t);
				Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
			}
			Debug.Log("TOTAL ERROR: " + totalError);
		}
	}

	void Start () {
		Train(8);
		Debug.Log("Test 0 0: " + CalcOutput(0,0));
		Debug.Log("Test 0 1: " + CalcOutput(0,1));
		Debug.Log("Test 1 0: " + CalcOutput(1,0));
		Debug.Log("Test 1 1: " + CalcOutput(1,1));		
	}
	
	void Update () {
		
	}
}
```
***Логическая операция OR (ИЛИ)***

Мною были установлены начальные значения:

![изображение](https://user-images.githubusercontent.com/114138439/204868203-aab3489d-dc15-4ae2-8cc2-6e77ae2f61a8.png)

Далее я проверяла обучаемость перцептрона, задавая разное количество эпох обучения. 
Значение Total Error отвечает за обучение перцептрона: если значение больше 0, значит модель не обучилась и нужно увеличить количество эпох, а если значение равно 0, значит модель успешно обучилась.
Перцептрон смог обучиться на 5-ой эпохе (прилагаю скрины уже второго запуска 5 эпох):

![изображение](https://user-images.githubusercontent.com/114138439/204868453-1c6e8af7-622f-4449-afc5-7d1fd41e59f3.png)
![изображение](https://user-images.githubusercontent.com/114138439/204868672-a50db1d9-a0c5-4b8b-9976-c6f5dde6174b.png)
![изображение](https://user-images.githubusercontent.com/114138439/204868761-8a030144-d2fa-4953-a8f9-2974f9b297cf.png)
![изображение](https://user-images.githubusercontent.com/114138439/204868824-09eac63e-2ea6-4686-a598-f55eed8ff8ed.png)
![изображение](https://user-images.githubusercontent.com/114138439/204868987-60bd345e-3620-4be6-927b-ce7d83b05a68.png)


После этого, в разультате скрипта, приложенного выше, мы обучили перцептрон и подали ему значения для проверки, и увидели, что он работает корректно:

![изображение](https://user-images.githubusercontent.com/114138439/204870244-0409bb66-8607-43bd-9d3e-8d887531b36d.png)


***Логическая операция AND (И)***

Устанавливаем значения:

![изображение](https://user-images.githubusercontent.com/114138439/204871505-f802fc8d-d7af-4f09-86e8-7a0350c54fcb.png)

Обучаем перцептрон. Видим, что он обучился за 7 эпох:

![изображение](https://user-images.githubusercontent.com/114138439/205033198-b2767ed0-c7d1-42e6-a94d-b1a32470ab84.png)
![изображение](https://user-images.githubusercontent.com/114138439/205033309-e890deff-5139-4c73-b195-01ccd690e503.png)
![изображение](https://user-images.githubusercontent.com/114138439/205033416-e2c69d20-1c9e-490d-aff3-f530b9ba79b6.png)

Проверяем корректность работы на тесты:

![изображение](https://user-images.githubusercontent.com/114138439/205033562-23eb49ce-2891-4032-93e6-e7cd85636499.png)

Видим, что перцептрон обучен успешно и работает корректно.


***Логическая операция NAND (ОТРИЦАНИЕ КОНЪЮНКЦИИ (AND))***

Устанавливаем значения на выход и на выход:

![изображение](https://user-images.githubusercontent.com/114138439/205033998-a356b2b2-3f41-4490-84e0-226cb4884ab6.png)


Обучаем:

![изображение](https://user-images.githubusercontent.com/114138439/205035129-c53c1641-5136-4bcb-b80f-4d4f2fbb54c7.png)
![изображение](https://user-images.githubusercontent.com/114138439/205035266-11c81018-c538-4f6c-9869-3c67102d1b7b.png)
![изображение](https://user-images.githubusercontent.com/114138439/205035340-5a5a709d-6478-4692-853a-0e0ed99cd681.png)

Видим, что перцептрон достиг Total Error = 0 за 6 эпох, и что тесты прошел, всё отработано корректно


***Логическая операция XOR (ИСКЛЮЧАЮЩЕЕ ИЛИ)***

Установила значения:

![изображение](https://user-images.githubusercontent.com/114138439/205036444-83d4966a-e8db-41e3-8b92-7b2a1ebd4e41.png)

Пыталась обучить на разном количестве эпох, но Total Error не становился нулем ни при каких заданных значениях Train(...)

Результаты при 5, 10, 50 эпохах соответственно:

![изображение](https://user-images.githubusercontent.com/114138439/205037042-47d03576-26cf-4e4c-bded-0e32db63d40e.png)  ![изображение](https://user-images.githubusercontent.com/114138439/205037234-d7ddeedf-6bbd-42cc-bb47-7e6b5e644959.png)
   ![изображение](https://user-images.githubusercontent.com/114138439/205037383-f23aed6c-da2a-43af-a988-6a1d20c6afc5.png)



## Задание 2




## Выводы



| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |
