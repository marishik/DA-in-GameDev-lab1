# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #3 выполнила:
- Алексеева Марина Геннадьевна
- РИ210931
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
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
Познакомиться с программными средствами для создания системы машинного обучения и ее интеграции в Unity.

## Задание 1
### Реализовать систему машинного обучения в связке Python - Google-Sheets – Unity.

Ход работы: 
1. Создала новый пустой 3D проект на Unity.
2. Скачала папку с ML агентом. Добавила ML Agent и .json – файлы:

![изображение](https://user-images.githubusercontent.com/114138439/204817107-c529cc1e-8ba1-4e3c-ac45-3c82776fb1c1.png)

3. Запустила Anaconda Prompt для возможности запуска команд через консоль. Написала серию команд для создания и активации нового ML-агента и для скачивания необходимых библиотек: mlagents 0.28.0 и torch 1.7.1:

![изображение](https://user-images.githubusercontent.com/114138439/204817227-db791471-3abf-40a7-abc9-7d15c7843d18.png)

4. В сцене создала плоскость, куб и сферу:
![изображение](https://user-images.githubusercontent.com/114138439/204815129-943a572d-29e0-4d53-b4ff-bcc8c44219ae.png)

C# код:
```

```
![изображение](https://user-images.githubusercontent.com/114138439/204817023-3a59a76c-5e5f-4297-abb4-32a047036d8e.png)


5. После добавления файла конфигурации нейронной сети в корень проекта, запустила работу ML-агента:
```
behaviors:
  RollerBall:
    trainer_type: ppo
    hyperparameters:
      batch_size: 10
      buffer_size: 100
      learning_rate: 3.0e-4
      beta: 5.0e-4
      epsilon: 0.2
      lambd: 0.99
      num_epoch: 3
      learning_rate_schedule: linear
    network_settings:
      normalize: false
      hidden_units: 128
      num_layers: 2
    reward_signals:
      extrinsic:
        gamma: 0.99
        strength: 1.0
    max_steps: 500000
    time_horizon: 64
    summary_freq: 10000
```
![изображение](https://user-images.githubusercontent.com/114138439/204816423-480f655c-a881-4988-a0ac-25b1c1dc0a3c.png)


## Задание 2

## Задание 3

### Должна ли величина loss стремиться к нулю при изменении исходных данных? Ответьте на вопрос, приведите пример выполнения кода, который подтверждает ваш ответ.


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
