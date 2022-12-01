# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
# 4. Лабораторная работа 4 - "Перцептрон".

Отчет по лабораторной работе #4 выполнил:
- Гаев Андрей Дмитриевич
- НМТ-213511 (АТ-04)
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
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.

## Цель работы
Познакомиться с программными средствами для создания системы машинного обучения и ее интеграции в Unity.


## Постановка задачи.
Использование перцептрона в Unity и Game Development'е, изучение его характерных свойтсв и особенностей.

## Задание 1. 
###  В проекте Unity реализовать перцептрон, который умеет производить вычисления OR | AND | NAND | XOR и дать комментарии о корректности роботы.
Код перцептрона
```C#
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
				//Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
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

Скрипт спроектирован таким образом, что для нас нет необходимости делать какие либо копии для изменения логики при обучении. Перцептрон будет переобучаться с нуля при запуске сцены для каждого GameObject, имеющего параметр Perceptron.cs в поле активных скриптов. Следовательно, нам остается только создать Empty-объекты к которым мы прикрепим скрипт, а после скорректировать вводные значение на вход и соответствующий им результат.

Получилось следующее:

![image](https://user-images.githubusercontent.com/90757310/204945627-c66214de-baaf-4e5c-9468-2653fd12b654.png)


А вот настройки одного из элементов, характеризующие присущую логическую операцию:

![image](https://user-images.githubusercontent.com/90757310/204945755-33055ae5-5c33-4db5-805d-06d8c0c3776d.png)

Запустим сцену: 

![image](https://user-images.githubusercontent.com/90757310/204945950-d03b2a85-5954-404f-9f80-a1a7c46c8d84.png)

Для прочих элементов все работает точно также.

Комментарии о корректности работы Train(16 -> 8):

- AND функционирует корекктно после 4-6 скармливаний тренировочной информации, иногда хватает двух.
- NAND функционирует корекктно после 4-8 скармливаний тренировочной информации.
- OR функционирует корекктно после 4-6 скармливаний тренировочной информации.
- XOR не функционирует, с увеличение Train результаты становятся только хуже - TOTAL ERROR увеличивается с каждой эпохой перцептрона.

Причины полученных результатов коррелируют с возможностями прецептрона. Первые логических оператора линейны, а последний такой зависимости не имеет. Поскольку перцептрон геометрически представляет из себя прямую с различными коэф. `aX+b` , то он не может обработать закономерности, представленные на картинке слева: 

![image](https://user-images.githubusercontent.com/90757310/204947276-3c7214dc-8b3e-4cd8-9b05-9367b2cb01af.png)

## Задание 2. 
###  Графики зависимости ошибок TE от кол-ва эпох.

![image](https://user-images.githubusercontent.com/90757310/204947874-636f1da8-9fce-494d-8297-82533000f25a.png)

Количество эпох зависит от изначального коэфицента веса(вознаграждения) и скорости редакции этого коэфицента. Второй фактор имеет некоторую хаотичность и при высоких значениях будет перепрыгивать нужную "Золотую середину", в результате количество эпох в среднем только увеличится, но появятся случаи, когда обучение происходить быстрее. Также кол-во эпох зависит от сложности самих логических операторов и вида зависимости, которую мы получим. 

Вот как мы влияем на веса в рамках нашей задачи.

```

void UpdateWeights(int j)
	{
		double error = ts[j].output - CalcOutput(j);
		totalError += Mathf.Abs((float)error);
		for(int i = 0; i < weights.Length; i++)
		{			
			weights[i] = weights[i] + error*ts[j].input[i]; // реакция на ошибку
		}
		bias += error; // отклонение
	}

```
```
void InitialiseWeights()
	{
		for(int i = 0; i < weights.Length; i++)
		{
			weights[i] = Random.Range(-1.0f,1.0f); //первое значение случайное
		}
		bias = Random.Range(-1.0f,1.0f);
	}
```


## Выводы
В рамках лабораторной работы 4 мы применили перцептор для реализации стандартных логических операторов и выяснили, что корректность исполненения будет присутствовать для функций линейного типа `aX+b`



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