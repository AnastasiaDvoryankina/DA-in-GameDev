# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнил(а):
- Дворянкина Анастасия Андреевна
- НМТ-211507 
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

## Лабораторная работа №2. Сбор, обработка и визуализация тестового набора данных.
## Цель работы
Цель работы: познакомиться с программными средствами для организции передачи данных между инструментами google, Python и Unity.

## Постановка задачи.
В данной лабораторной работе на языке python будет реализован функционал, позволяющий генерировать стоимость товара (ресурса или игрового объекта) в виде набора данных. Созданный набор данных будет передан в google-таблицу с целью возможности дальнейшего их наглядного представляния и оптимизации. Также в этой лабораторной работе на движке Unity будет реализован функционал, позволяющий воспроизводить аудио-файлы со звуковой информацией в зависимости от значений входного набора данных из таблицы.
Позднее, в следующей лабораторной работе мы будем изменять стоимость указанного товара (ресурса или игрового объекта) через ML-агент. Далее стоимость товара будет меняться  в зависимости от прочих переменных (например, колличества собранного золота, скорости добычи сырья, скорости передвижения неигрового персонажа от рудника до города и пр). На основе указанных переменных мы научим ML-агент не выходить за пределы темпа инфляции.

## Задание 1. 
### Реализовать совместную работу и передачу данных в связке Python - Google-Sheets – Unity. При выполнении задания используйте видео-материалы и исходные данные, предоставленные преподавателя курса.
- В облачном сервисе google console подключить API для работы с google sheets и google drive.
- Реализовать запись данных из скрипта на python в google-таблицу. Данные описывают изменение темпа инфляции на протяжении 11 отсчётных периодов, с учётом стоимости игрового объекта в каждый период.
- Создать новый проект на Unity, который будет получать данные из google-таблицы, в которую были записаны данные в предыдущем пункте.
- Написать функционал на Unity, в котором будет воспризводиться аудио-файл в зависимости от значения данных из таблицы.

Step 1. Создаем Goodle Sheets; аккаунт-сервис, который будет редактором нашей таблицы, и, посредством которого мы будем направлять результаты работы скрипта или модели в таблицу; ключ для подключения аккаунта сервиса, подгружаем его. Также скачаем звуковые и JSON файлы с яндекс диска.
- В итоге получаем:
![image](https://user-images.githubusercontent.com/90757310/195180157-ec3f417b-c61a-4544-bef3-84c6c192fdad.png)

Step 2. Пишем скриптовую часть нашей системы на python. Используем библиотеку "gspread" для взаимодействия с гугл таблицами и "numpy" для генерации наших даный для внесения в таблицу. Подлюкчаем API-ключ в скрипт, теперь он можент обращаться к нашей таблице и, если имеет роль редактора, которую дали ранее, то может ее редактировать, т.е. вносить наши сгенерированные значения. Проверяем заполненость таблицы, переходим к части считывания данных.
- Скрипт
![image](https://user-images.githubusercontent.com/90757310/195180597-63009a8e-a5f1-405f-bd52-d2de5afdfbe9.png)
- Форма запроса к таблице
![image](https://user-images.githubusercontent.com/90757310/195180726-77cb0494-c4a6-4469-b666-f9cbdf12c2c1.png)
- Полученные значения

![image](https://user-images.githubusercontent.com/90757310/195180791-8c595fa7-e902-4108-887f-0dcadab45213.png)

Step 3. Создаем проект Unity, в который добавляем скрипт и EmptyObject, которые в коллаборации создадут визуализацию наших данных. Или Аудиозацию. \_^-^_/ Редактируем скрипт, внося в него функционал воспроизведения звуковых дорожек, Веб-запрос с адресом нашей таблицы и ключа подключения к ней, который мы создали в Credentials. Если запрос написан правильно, то перейдя по ссылке мы получем версию нашей таблицы для скрипта. Далее мы пишем простенькую логику для определения величины серипта в ячейке, если запрос на ячейку сформулирован правильно, то мы получим нужную реакцию. Если наш запрос к ячейке неправильно написан или ее содержимое невозможно "распарсить", то мы ыполучим крит. Фиксируем результат
- EmtyObject с наложенным на него скриптом

![image](https://user-images.githubusercontent.com/90757310/195183005-62fe46e4-d2b5-4ded-99c9-cb8799ad1817.png)

- Запрос к таблице
![image](https://user-images.githubusercontent.com/90757310/195183232-575f9f82-1529-415c-a65b-db5a07861c75.png)

- Логика воспроизведения звуковых дорожек
![image](https://user-images.githubusercontent.com/90757310/195183402-84fe2003-6373-42ff-b4d1-f08a92f77069.png)
Обнаружила ошибку, которая не воспроизводила последнюю строчку в столбце. Элемент i != dataSet.Count говорит о том, что мы воспроизводим звук, если наш счетчик не больше колва строк. Однако в python списки начинаются с 1го элемента, а в C# с нулевого, поэтому последняя строчка в интерпритации на C# не отоброжалось. Я заметила это и исправила ситуацию путем добавления к крайнему положению счетчика единицу.

Последнее значение в таблице ![image](https://user-images.githubusercontent.com/90757310/195187106-f7ee51ae-d65e-4c6d-8e7b-8b5925812a58.png)

- Было ![image](https://user-images.githubusercontent.com/90757310/195186897-b3c4b167-da2f-468b-814a-956896cf4287.png)
и ![image](https://user-images.githubusercontent.com/90757310/195187211-e11d190c-15fd-4042-9b8b-65332c632553.png)
- Стало ![image](https://user-images.githubusercontent.com/90757310/195187429-67f07c02-f73d-486a-880a-4381759c1fe5.png)
 и ![image](https://user-images.githubusercontent.com/90757310/195187490-5ff438cc-ef0c-4ff7-9cf4-8ca808ec2c8c.png)

Ну и по скольку метод Update работает сразу, как только готов новый кадр, то мы получаем неограниченное число вызовов информации из гугл таблицы, надо бы как-то его ограничить. Например добавить, что ячейка в принципе имеет содержимое, т.е. [Содержимое] != null или [Статус подключения к таблице] == true, что работает с базами данных, но мои попытки воспроизвести этот метод потерпели крах. Хотелось бы ответов, но у индусов нет ролика на эту тему. А вот статья на хабре имеет такой функционал, однако он тоже не подходит. Мои полномочия "всё" на сегодняшний день, придется вернуться к вопросу, когда знаний будет хватать для его решения.

- Процедуры вызова дорожек

![image](https://user-images.githubusercontent.com/90757310/195183351-d7286876-3e5e-4357-bc9b-d1625e0ef3aa.png)

- Гифка работы скрипта

![](https://github.com/mhehet18jiet/DA-in-GameDev/blob/main/DAinGD%20Lab%232/Lab2GIF%20(2).gif)

## Задание 2
Присоединим прогресс обучения нашей модели линейной регрессии из Лабораторной 1 к GoogleSheets
```py
import numpy as np
import matplotlib.pyplot as plt
import gspread
import numpy.random

import numpy as np
import matplotlib.pyplot as plt
import gspread
import numpy.random

x = [3,21,22,34,54,34,55,67,89,99]
x = np.array(x)
y = [2,22,24,65,79,82,55,130,150,199]
y = np.array(y)
gc = gspread.service_account(filename='unitysound-b3b9dd2033d6.json')
sh = gc.open("UnitySound")
plt.scatter(x,y)
plt.show()
def model(a, b, x):
    return a*x + b

def loss_function(a, b, x, y):
    num = len(x)
    prediction=model(a,b,x)
    return (0.5/num) * (np.square(prediction-y)).sum()

def optimize(a,b,x,y):
    num = len(x)
    prediction = model(a,b,x)
    da = (1.0/num) * ((prediction -y)*x).sum()
    db = (1.0/num) * ((prediction -y).sum())
    a = a - Lr*da
    b = b - Lr*db
    return a, b

def iterate(a,b,x,y,times):
    for i in range(times):
        a,b = optimize(a,b,x,y)
    return a,b
a = np.random.rand(1)
print(a)
b = np.random.rand(1)
print(b)
Lr = 0.000001
for j in range(1,6):
    if j == 1:
        print("j = ", j)
        a, b = iterate(a, b, x, y, 1)
        prediction = model(a, b, x)
        loss = loss_function(a, b, x, y)
        print(a, b, loss)
        plt.scatter(x, y)
        plt.plot(x, prediction)
        plt.show()
        _loss = str(loss)
        _loss = _loss.replace('.', ',')
        sh.sheet1.update(('A' + str(j)), str(j))
        sh.sheet1.update(('B' + str(j)), str(_loss))
    if j == 2:
        print("j = ", j)
        a, b = iterate(a, b, x, y, 100)
        prediction = model(a, b, x)
        loss = loss_function(a, b, x, y)
        print(a, b, loss)
        plt.scatter(x, y)
        plt.plot(x, prediction)
        plt.show()
        _loss = str(loss)
        _loss = _loss.replace('.', ',')
        sh.sheet1.update(('A' + str(j)), str(j))
        sh.sheet1.update(('B' + str(j)), str(_loss))
    if j == 3:
        print("j = ", j)
        a, b = iterate(a, b, x, y, 250)
        prediction = model(a, b, x)
        loss = loss_function(a, b, x, y)
        print(a, b, loss)
        plt.scatter(x, y)
        plt.plot(x, prediction)
        plt.show()
        _loss = str(loss)
        _loss = _loss.replace('.', ',')
        sh.sheet1.update(('A' + str(j)), str(j))
        sh.sheet1.update(('B' + str(j)), str(_loss))
    if j == 4:
        print("j = ", j)
        a, b = iterate(a, b, x, y, 300)
        prediction = model(a, b, x)
        loss = loss_function(a, b, x, y)
        print(a, b, loss)
        plt.scatter(x, y)
        plt.plot(x, prediction)
        plt.show()
        _loss = str(loss)
        _loss = _loss.replace('.', ',')
        sh.sheet1.update(('A' + str(j)), str(j))
        sh.sheet1.update(('B' + str(j)), str(_loss))
    if j == 5:
        print("j = ", j)
        a, b = iterate(a, b, x, y, 15000)
        prediction = model(a, b, x)
        loss = loss_function(a, b, x, y)
        print(a, b, loss)
        plt.scatter(x, y)
        plt.plot(x, prediction)
        plt.show()
        _loss = str(loss)
        _loss = _loss.replace('.', ',')
        sh.sheet1.update(('A' + str(j)), str(j))
        sh.sheet1.update(('B' + str(j)), str(_loss))
```

- Запустим и посмотрим на результат.

![Start](https://user-images.githubusercontent.com/90757310/200011397-c2e18252-7276-4efe-b5c7-a6ccfc5d2631.gif)

## Задание 3

- Напишем скрипт с подключением к новой гугл таблице. Также адаптируем логику под изменение величиныы loss, которая свидетельствует о результате обучения нашей модели. Относительно loss выстроим алгоритм воспроизведения звуков с тремя состояниями: 1) Начало обучения, 2) Середина обучения, 3)Модель обучена.

```C#
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

    void Start()
    {
        StartCoroutine(GoogleSheets());
    }

    void Update()
    {
     
        if (dataSet["Mon_" + i.ToString()] >= 3000 & statusStart == false & i != (dataSet.Count + 1) )
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] < 3000 & dataSet["Mon_" + i.ToString()] > 200 & statusStart == false & i != (dataSet.Count + 1) )//1/3
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] < 200 & statusStart == false & i != (dataSet.Count + 1)) // done
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
     
    }

    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1wDAIR_6i90CYx65ufw8R-w44TIaKR_E5XS9cH6sc96U/values/Лист1?key=AIzaSyCn2PlrP0HeI-BwONiukz37VBrXIg6gjNs");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[1]));
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
        yield return new WaitForSeconds(4);
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

- Создадим свои звуковые файлы и добавим их в компонент нашего скрипта.

Демонстрация

![End](https://user-images.githubusercontent.com/90757310/200013358-990af31e-0030-47b3-825f-0dfdbea1a7cf.gif)


## Выводы
Мной была создана система генерации - записи - считывания информации с помощью небольшого скрипта на пайтоне, запись сгенерированных данных в гугл-таблицу и считывание их в юнити. Данная система может быть использована как функционал отладки при работе с данными в играх, при анализе фондовых рынков на основании выводов обученной модели и прочих манипуляциях с данными. Важным фактом является возможность взаимодействия машинных систем и баз данных, которые представлены в нашей работе в виде гугл таблицы. Благодарю за внимание.

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
