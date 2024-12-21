# Работа #4. ПЕРЦЕПТРОН
Отчет по лабораторной работе #4 выполнил:
- Афонасьев Артём Ильич
- РИ-230948

Отметка о выполнении заданий:

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 0 |
| Задание 2 | * | 0 |
| Задание 3 | # | 0 |

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
- Код реализации выполнения задания. Визуализация результатов выполнения.
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Достичь понимания принципов работы перцептрона через практическую реализацию, визуализацию и анализ его работы на примере простых логических операций.

## Задание 1
В проекте Unity реализовать перцептрон, который умеет производить вычисления:
- OR | дать комментарии о корректности работы
- AND | дать комментарии о корректности работы
- NAND | дать комментарии о корректности работы
- XOR | дать комментарии о корректности работы
```c#
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
    public double[] input;
    public double output;
}

public class Perceptron : MonoBehaviour
{ 
    public TrainingSet[] ts;
    double[] weights = { 0, 0 };
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
        double dp = DotProductBias(weights, ts[i].input);
        if (dp > 0) return (1);
        return (0);
    }

    void InitialiseWeights()
    {
        for (int i = 0; i < weights.Length; i++)
        {
            weights[i] = UnityEngine.Random.Range(-1.0f, 1.0f);
        }
        bias = UnityEngine.Random.Range(-1.0f, 1.0f);
    }

    void UpdateWeights(int j)
    {
        double error = ts[j].output - CalcOutput(j);
        totalError += Mathf.Abs((float)error);
        for (int i = 0; i < weights.Length; i++)
        {
            weights[i] = weights[i] + error * ts[j].input[i];
        }
        bias += error;
    }

    double CalcOutput(double i1, double i2)
    {
        double[] inp = new double[] { i1, i2 };
        double dp = DotProductBias(weights, inp);
        if (dp > 0) return (1);
        return (0);
    }

    void Train(int epochs)
    {
        InitialiseWeights();

        for (int e = 0; e < epochs; e++)
        {
            totalError = 0;
            for (int t = 0; t < ts.Length; t++)
            {
                UpdateWeights(t);
                Debug.Log($"{t}{e} W1: {Math.Round(weights[0],3)} W2:  {Math.Round(weights[1],3)} B: {Math.Round(bias,3)}");
            }
            Debug.Log("TOTAL ERROR: " + totalError);
        }
    }

    void Start()
    {
        Train(8);
        Debug.Log("Test 0 0: " + CalcOutput(0, 0));
        Debug.Log("Test 0 1: " + CalcOutput(0, 1));
        Debug.Log("Test 1 0: " + CalcOutput(1, 0));
        Debug.Log("Test 1 1: " + CalcOutput(1, 1));
    }

    void Update()
    {

    }
}
```

**OR** (ИЛИ):
  - Операция OR является линейно разделимой задачей.
  - **Комментарий**: Перцептрон корректно работает для OR, так как способен найти линейную разделяющую гиперплоскость. Код успешно обучается и предсказывает эту операцию.

![image](https://github.com/user-attachments/assets/e86f97db-d265-4850-889c-dc503a59a93c)
**AND** (И):
   - Операция AND  также является линейно разделимой задачей.
   - **Комментарий**: Перцептрон корректно работает для AND, так как задача линейно разделима. Код справляется с обучением и предсказанием.
![image](https://github.com/user-attachments/assets/ef4da56e-6208-45ca-9523-e54ada633aef)

**NAND** (НЕ И):
   - Операция NAND является инверсией AND. Она тоже линейно разделима.
   - **Комментарий**: Перцептрон корректно работает для NAND, так как задача также линейно разделима, и подход с одним слоем позволяет корректно обучить модель.
![image](https://github.com/user-attachments/assets/b3879abb-baee-4105-b47a-a1b07fb067fc)

**XOR** (исключающее ИЛИ):
   - Операция XOR не является линейно разделимой задачей. Например, при вводах (0,0) и (1,1) выходы равны 0, а при (0,1) и (1,0) равны 1.
   - **Комментарий**: Перцептрон не способен корректно обучиться XOR из-за его линейно неразделимого характера. Для решения требуется многослойная нейронная сеть или дополнительный уровень нелинейности.
![image](https://github.com/user-attachments/assets/96a2aa70-33cb-40fd-81ff-eb2538758c21)

  

## Задание 2
Построить графики зависимости количества эпох от ошибки обучения. Указать от чего зависит необходимое количество эпох обучения.

Изменим код
```c#
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
    public double[] input;
    public double output;
}

public class Perceptron : MonoBehaviour
{ 
    public TrainingSet[] ts;
    double[] weights = { 0, 0 };
    double bias = 0;
    double totalError = 0;
    float graphOffset;

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
        double dp = DotProductBias(weights, ts[i].input);
        if (dp > 0) return (1);
        return (0);
    }

    void InitialiseWeights()
    {
        for (int i = 0; i < weights.Length; i++)
        {
            weights[i] = UnityEngine.Random.Range(-1.0f, 1.0f);
        }
        bias = UnityEngine.Random.Range(-1.0f, 1.0f);
    }

    void UpdateWeights(int j)
    {
        double error = ts[j].output - CalcOutput(j);
        totalError += Mathf.Abs((float)error);
        for (int i = 0; i < weights.Length; i++)
        {
            weights[i] = weights[i] + error * ts[j].input[i];
        }
        bias += error;
    }

    double CalcOutput(double i1, double i2)
    {
        double[] inp = new double[] { i1, i2 };
        double dp = DotProductBias(weights, inp);
        if (dp > 0) return (1);
        return (0);
    }

    void Train(int epochs, string operationLabel, Dictionary<string, List<double>> allErrors)
    {
        InitialiseWeights();
        List<double> errorPerEpoch = new List<double>(); // Хранилище ошибок

        for (int e = 0; e < epochs; e++)
        {
            totalError = 0;
            for (int t = 0; t < ts.Length; t++)
            {
                UpdateWeights(t);
            }
            errorPerEpoch.Add(totalError);
        }

        // Сохраняем ошибки для данной операции
        allErrors[operationLabel] = errorPerEpoch;
    }

    void Start()
    {
        Dictionary<string, List<double>> allErrors = new Dictionary<string, List<double>>();

        // AND
        ts = new TrainingSet[]
        {
        new TrainingSet { input = new double[] { 0, 0 }, output = 0 },
        new TrainingSet { input = new double[] { 0, 1 }, output = 0 },
        new TrainingSet { input = new double[] { 1, 0 }, output = 0 },
        new TrainingSet { input = new double[] { 1, 1 }, output = 1 },
        };
        Train(20, "AND", allErrors);

        // OR
        ts = new TrainingSet[]
        {
        new TrainingSet { input = new double[] { 0, 0 }, output = 0 },
        new TrainingSet { input = new double[] { 0, 1 }, output = 1 },
        new TrainingSet { input = new double[] { 1, 0 }, output = 1 },
        new TrainingSet { input = new double[] { 1, 1 }, output = 1 },
        };
        Train(20, "OR", allErrors);

        // NAND
        ts = new TrainingSet[]
        {
        new TrainingSet { input = new double[] { 0, 0 }, output = 1 },
        new TrainingSet { input = new double[] { 0, 1 }, output = 1 },
        new TrainingSet { input = new double[] { 1, 0 }, output = 1 },
        new TrainingSet { input = new double[] { 1, 1 }, output = 0 },
        };
        Train(20, "NAND", allErrors);

        // XOR
        ts = new TrainingSet[]
        {
        new TrainingSet { input = new double[] { 0, 0 }, output = 0 },
        new TrainingSet { input = new double[] { 0, 1 }, output = 1 },
        new TrainingSet { input = new double[] { 1, 0 }, output = 1 },
        new TrainingSet { input = new double[] { 1, 1 }, output = 0 },
        };
        Train(20, "XOR", allErrors);

        // Сохранение всех данных в один файл
        SaveAllErrorsToCSV(allErrors, "TrainingErrors");
    }

    void SaveAllErrorsToCSV(Dictionary<string, List<double>> allErrors, string fileName)
    {
        string filePath = Application.dataPath + "/" + fileName + ".csv"; // Путь к файлу в папке Assets
        List<string> lines = new List<string>();

        // Заголовок с использованием ;
        lines.Add("Operation;Epoch;Error");

        // Данные
        foreach (var entry in allErrors)
        {
            string operation = entry.Key;
            List<double> errors = entry.Value;

            for (int i = 0; i < errors.Count; i++)
            {
                lines.Add($"{operation};{i};{errors[i]}"); // Используем ; как разделитель
            }
        }

        // Сохранение в файл
        System.IO.File.WriteAllLines(filePath, lines);
        Debug.Log($"Все данные сохранены в: {filePath}");
    }

    void Update()
    {

    }
}
```
  ![image](https://github.com/user-attachments/assets/0ddd63e7-5f30-4abb-a7dd-39ab7afc8634)
#### Влияющие факторы на количество эпох:
- Начальная инициализация весов:
Если начальные веса близки к правильным значениям, обучение будет быстрее.
При случайной инициализации может потребоваться больше эпох.
- Скорость обучения (learning rate):
Большие изменения весов за шаг ускоряют обучение, но могут привести к неустойчивости.
Малые изменения делают обучение более стабильным, но требуют больше эпох.
- Сложность задачи:
Линейно разделимые задачи (AND, OR, NAND) сходятся быстрее.
Нелинейные задачи (XOR) не решаются в рамках данной архитектуры.
- Шум в данных:
При наличии ошибок или шумов в обучающих данных перцептрон может потребовать больше времени или вообще не сойтись.
- Размер набора данных:
Чем больше примеров для обучения, тем больше эпох может потребоваться.
- Форма функции ошибки:
Использование общей ошибки в сумме может быть недостаточно чувствительным, если веса незначительно обновляются.



## Выводы
В ходе работы были реализованы логические операции с использованием перцептрона, построены зависимости обучения. Исследование показало, что перцептрон успешно справляется с линейно разделимыми задачами, но требует усложнения архитектуры для нелинейных операций.


| Plugin | README |
| ------ | ------ |
| GitHub | [plugins/github/README.md][PlGh] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
