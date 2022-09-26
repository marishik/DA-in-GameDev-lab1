# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Алексеева Марина Геннадьевна
- РИ210931
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | # | 20 |
| Задание 3 | # | 20 |

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
Ознакомиться с основными операторами языка Python на примере реализации линейной регрессии.

## Задание 1
Python:
```
print("Hello World!")
```
![изображение](https://user-images.githubusercontent.com/114138439/191996456-10bb97fc-607c-4f90-8567-d8c4703b3951.png)
![изображение](https://user-images.githubusercontent.com/114138439/191996598-f1c5d347-7ada-402b-aa2b-26e283d58cbe.png)

Unity:
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
public class HelloWorld : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        print("Hello World!");
    }

    // Update is called once per frame
    void Update()
    {
        
    }
}
```
![изображение](https://user-images.githubusercontent.com/114138439/191997041-45e8f9a7-b072-40ef-a20e-5f6e5292b6e4.png)

## Задание 2
Пошагово выполнить каждый пункт раздела "ход работы" с описанием и примерами реализации задач:

Произведена подготовка данных для работы с алгоритмом линейной регрессии. 10 видов данных были установлены случайным образом, и данные находились в линейной зависимости. Данные преобразуются в формат массива, чтобы их можно было вычислить напрямую при использовании умножения и сложения.
Определены связанные функции. 
Функция модели: определяет модель линейной регрессии wx+b. 
Функция потерь: функция потерь среднеквадратичной ошибки. 
Функция оптимизации: метод градиентного спуска для нахождения частных производных w и b

Код при итерации №1 :
```
#Import the required modules, numpy for calculation, and Matplotlib for drawing
!pip install numpy
!pip install matplotlib
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline

#define data, and change list to array
x = [3, 21, 22, 34, 54, 34, 55, 67, 89,99]
x = np.array(x)

y = [2, 22, 24, 65, 79, 82, 55, 130, 150, 199]
y = np.array(y)

#Show the effect of a scatter plot
plt.scatter(x,y)

#The basic linear regression model is wx+b, and since this is a two-dimensional space, the model is ax+b
def model(a, b, x):
    return a*x + b

#The most commonly used loss function of linear regresstion model is the loss function of mean, variance difference
def loss_function(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    return (0.5/num)*(np.square(prediction - y)).sum()

#The optimization function mainly USES partial derivatives to update two parameters a and b
def optimize (a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    #Update the values of A and B by finding the partial derivatives of the loss function on a and b
    da = (1.0/num)*((prediction - y)*x).sum()
    db = (1.0/num)*((prediction - y).sum())
    a = a - Lr*da
    b = b - Lr*db
    return a, b

#iterated function, return a and b
def iterate (a, b, x, y, times):
    for i in range (times):
        a, b = optimize(a, b, x, y)
    return a, b

#Initialize parameters and display
a = np.random.rand(1)
print (a)
b = np.random.rand(1)
print(b)
Lr = 0.000001

#For the first iteration, the parameter values, losses, and visualization after the iteration are displayed
a, b = iterate(a, b, x, y, 1)
prediction = model(a, b, x)
loss = loss_function(a, b, x, y)
print(a, b, loss)
plt.scatter(x, y)
plt.plot(x, prediction)
```
Результат при 1 итерации:
![изображение](https://user-images.githubusercontent.com/114138439/192345977-1b6f856d-ad91-4c20-9871-0a582f1bfd33.png)


2 итерация:
![изображение](https://user-images.githubusercontent.com/114138439/192346470-36ec7918-e090-4446-989f-3cda54d95c4a.png)


3 итерация:
![изображение](https://user-images.githubusercontent.com/114138439/192346606-9433a410-8e31-4e07-bfcc-bd4d5accd9d6.png)

4 итерация:
![изображение](https://user-images.githubusercontent.com/114138439/192346782-7fcd7509-7c9a-4e6e-a297-6eba384251df.png)

5 итерация:
![изображение](https://user-images.githubusercontent.com/114138439/192346996-ea6c5ea6-cf19-43ef-986b-98dc6859e46c.png)

10000 итерация:
![изображение](https://user-images.githubusercontent.com/114138439/192351941-fd34f61d-19a0-400d-be27-0dd70d57e3df.png)


## Задание 3
### Какова роль параметра Lr? Ответьте на вопрос, приведите пример выполнения кода, который подтверждает ваш ответ. В качестве эксперимента можете изменить значение параметра.
![изображение](https://user-images.githubusercontent.com/114138439/192356210-c9908832-e0d0-495f-a0e1-62ecaf6cde88.png)
![изображение](https://user-images.githubusercontent.com/114138439/192355561-fe8ead55-9782-420f-a7e6-bb8a540774b1.png)
![изображение](https://user-images.githubusercontent.com/114138439/192355688-4da07df5-a172-422d-894f-2e5831f2910f.png)
![изображение](https://user-images.githubusercontent.com/114138439/192355930-0bca41a2-17a9-4066-b66c-b92b8006fa39.png)


- Перечисленные в этом туториале действия могут быть выполнены запуском на исполнение скрипт-файла, доступного [в репозитории](https://github.com/Den1sovDm1triy/hfss-scripting/blob/main/ScreatingSphereInAEDT.py).
- Для запуска скрипт-файла откройте Ansys Electronics Desktop. Перейдите во вкладку [Automation] - [Run Script] - [Выберите файл с именем ScreatingSphereInAEDT.py из репозитория].

```py

import ScriptEnv
ScriptEnv.Initialize("Ansoft.ElectronicsDesktop")
oDesktop.RestoreWindow()
oProject = oDesktop.NewProject()
oProject.Rename("C:/Users/denisov.dv/Documents/Ansoft/SphereDIffraction.aedt", True)
oProject.InsertDesign("HFSS", "HFSSDesign1", "HFSS Terminal Network", "")
oDesign = oProject.SetActiveDesign("HFSSDesign1")
oEditor = oDesign.SetActiveEditor("3D Modeler")
oEditor.CreateSphere(
	[
		"NAME:SphereParameters",
		"XCenter:="		, "0mm",
		"YCenter:="		, "0mm",
		"ZCenter:="		, "0mm",
		"Radius:="		, "1.0770329614269mm"
	], 
)

```

## Выводы

Абзац умных слов о том, что было сделано и что было узнано.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
