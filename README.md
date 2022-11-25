# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]



Отчет по лабораторной работе #4 выполнил:
- Исмагилов Денис Рустамович
- РИ210945
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




## Цель работы
 - Реализовать работу перцептрона в Unity.


## Задание 1
 - Научить перцептрон вычислять операции: дизюнкция, конъюнкция, отрицание конъюнкции, строгая дизъюнкция 

## Ход работы:
Создадим сцену в Unity. Создадим в ней пустой объект и назовём его "Obj". Добавим к пустому объекту скрипт "Perceptron".
- Перцептрон - компьютерная модель восприятия информации мозгом. Перцептрон состоит из трёх типов элементов, а именно: поступающие от датчиков сигналы передаются ассоциативным элементам, а затем реагирующим элементам. Таким образом, перцептроны позволяют создать набор «ассоциаций» между входными стимулами и необходимой реакцией на выходе.

Напишем скрипт перцептрона:
```c#
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
У объекта, в компоненте скрипт, появился список тренировок.


![1](https://user-images.githubusercontent.com/106258306/202890445-4d574460-a1a7-4ae7-aca5-a2954be781a4.png)

Увеличим количество элементов до 4, нажав на "+" 4 раза или вместо 0 написав 4. У каждого элемента на вход добавим два элемента, нажав на "+" под строкой "input". Раскроем элементы, нажав на треугольник рядом с названиями элементов.


![2](https://user-images.githubusercontent.com/106258306/202890660-592108cc-e737-4d38-9e9c-9fb5b28ccbce.png)


Реализуем логические операции, используя 0 и 1.
## Дизъюнкция

| a | b | result |
|-|-|------|
|0|0|0|
|0|1|1|
|1|0|1|
|1|1|1|

Зададим в скрипте соответствующие таблице значения.


![3](https://user-images.githubusercontent.com/106258306/202890989-403ac314-3616-4a1a-afce-1ecf81e19709.png)


Запустим сцену. В консоли вывелись значения. Перейдём в вкладку console:

![4](https://user-images.githubusercontent.com/106258306/202891116-c3fd9294-02da-4fab-8e3e-a624989874f4.png)

Систематизируем результаты:

|Эпоха|Кол-во ошибок|
|-----|-------------|
|1|2|
|2|1|
|3|0|
|4|0|
|5|0|
|6|0|
|7|0|
|8|0|

Посмотрим на результаты обучения, пролистав консоль вниз:

![5](https://user-images.githubusercontent.com/106258306/202891644-27791816-4396-4524-b21c-a65667228bcb.png)

Результаты соответствуют значениям в таблице, приведённой в описании операции. Перцептрон успешно обучился.

## Конъюнкция

| a | b | result |
|-|-|------|
|0|0|0|
|0|1|0|
|1|0|0|
|1|1|1|

Зададим в скрипте соответствующие таблице значения.


![6](https://user-images.githubusercontent.com/106258306/202891845-57875794-31e6-47a1-8e0f-f3cde1028dcd.png)


Запустим сцену. В консоли вывелись значения. Перейдём в вкладку console:

![7](https://user-images.githubusercontent.com/106258306/202891899-698a4846-9f78-426d-ba9b-84ebcb5492f3.png)

Систематизируем результаты:

|Эпоха|Кол-во ошибок|
|-----|-------------|
|1|3|
|2|2|
|3|3|
|4|2|
|5|1|
|6|0|
|7|0|
|8|0|

Посмотрим на результаты обучения, пролистав консоль вниз:

![8](https://user-images.githubusercontent.com/106258306/202891915-99670a82-f9c9-4f04-b062-4f138b12bb70.png)

Результаты соответствуют значениям в таблице, приведённой в описании операции. Перцептрон успешно обучился.

## Отрицание конъюнкции

| a | b | result |
|-|-|------|
|0|0|1|
|0|1|1|
|1|0|1|
|1|1|0|

Зададим в скрипте соответствующие таблице значения.


![9](https://user-images.githubusercontent.com/106258306/202892227-9698b46a-3a2c-43d2-9d03-27e7b0c817a9.png)


Запустим сцену. В консоли вывелись значения. Перейдём в вкладку console:

![10](https://user-images.githubusercontent.com/106258306/202892230-3bf4dd30-c8ab-4d20-80ef-05256fed8e2b.png)

Систематизируем результаты:

|Эпоха|Кол-во ошибок|
|-----|-------------|
|1|2|
|2|3|
|3|3|
|4|2|
|5|1|
|6|0|
|7|0|
|8|0|

Посмотрим на результаты обучения, пролистав консоль вниз:

![11](https://user-images.githubusercontent.com/106258306/202892216-32cbe0d7-6dc5-4e1a-93c0-72113081c032.png)

Результаты соответствуют значениям в таблице, приведённой в описании операции. Перцептрон успешно обучился.

## Строгая дизъюнкция

| a | b | result |
|-|-|------|
|0|0|0|
|0|1|1|
|1|0|1|
|1|1|0|

Зададим в скрипте соответствующие таблице значения.


![9](https://user-images.githubusercontent.com/106258306/202892227-9698b46a-3a2c-43d2-9d03-27e7b0c817a9.png)


Запустим сцену. В консоли вывелись значения. Перейдём в вкладку console:

![12](https://user-images.githubusercontent.com/106258306/202892469-41ab6403-d6bc-4e9e-8e43-79e3dc5ed116.png)

Систематизируем результаты:

|Эпоха|Кол-во ошибок|
|-----|-------------|
|1|3|
|2|2|
|3|3|
|4|4|
|5|4|
|6|4|
|7|4|
|8|4|

Посмотрим на результаты обучения, пролистав консоль вниз:

![14](https://user-images.githubusercontent.com/106258306/202892572-101116eb-daf6-43e2-aab8-362ba350f4cd.png)

Результаты не соответствуют значениям в таблице, приведённой в описании операции. Перцептрон не обучился, так как на момент 8-ой эпохи кол-во ошибок не равно нулю. 

Перцпетрон неспособен обучится строгой дизъюнкции, так как в нём всего 1 нейрон. Так что сколько бы мы не увеличивали количество эпох обучения, он не сможет дать нам верный релузьтат.

## Задание 2
 - Построить график зависимости количества эпох от ошибки обучения 

## Ход работы:
Скопируем данные результатов обучения и построим по ним график. Используем для этого Python и библиотеку matplotlib.
```python
import matplotlib.pyplot as plt

era = [1, 2, 3, 4, 5, 6, 7, 8]

OR = [2, 1, 0, 0, 0, 0, 0, 0]
AND = [3, 2, 3, 2, 1, 0, 0, 0]
NAND = [2, 3, 3, 2, 1, 0, 0, 0]
XOR = [3, 2, 3, 4, 4, 4, 4, 4]

plt.subplot(2, 2, 1)
plt.plot(era, OR)
plt.title('OR', fontsize=25)
plt.xlabel('Эпохи', fontsize=18)
plt.ylabel('Кол-во ошибок', fontsize=16)

plt.subplot(2, 2, 2)
plt.plot(era, AND)
plt.title('AND', fontsize=25)
plt.xlabel('Эпохи', fontsize=18)
plt.ylabel('Кол-во ошибок', fontsize=16)

plt.subplot(2, 2, 3)
plt.plot(era, NAND)
plt.title('NAND', fontsize=25)
plt.xlabel('Эпохи', fontsize=18)
plt.ylabel('Кол-во ошибок', fontsize=16)

plt.subplot(2, 2, 4)
plt.plot(era, XOR)
plt.title('XOR', fontsize=25)
plt.xlabel('Эпохи', fontsize=18)
plt.ylabel('Кол-во ошибок', fontsize=16)

plt.subplots_adjust(wspace=0.3, hspace=0.8)
plt.show()
```
При выполнении скрипта получится следующие графики:

![era](https://user-images.githubusercontent.com/106258306/203932099-a349f346-960d-4073-92eb-b2bbf817f409.png)

