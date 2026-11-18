# Лабораторная работа № 2. Классификация и нейронные сети

**[Открыть выполненный отчёт в Jupyter Notebook](lab2.ipynb)** · [HTML-версия для браузера](report.html)

Задача — распознать гамма-сигнал на фоне адронных событий по десяти характеристикам вспышки. В работе сравниваются четыре варианта наивного Байеса, дерево решений, LDA, SVM, k-NN и полносвязная сеть TensorFlow/Keras. [Описание данных и источник](data/README.md)

На тесте из 3 781 события MLP дала наибольший ROC-AUC (0,9213). У SVM получился близкий ROC-AUC (0,9185) и лучший F1 при стандартном пороге (0,9038). Парный 95%-й бутстрэп-интервал для разности ROC-AUC включает ноль.

## Материалы

- В ноутбуке сохранены вычисления, таблицы, ROC-кривые, диаграммы четырёх метрик и графики обучения.
- В разделе 3.1 показаны корреляции десяти признаков на обучающей выборке и проверено соотношение двух характеристик яркости. [Матрица корреляций](results/figures/01b_correlations.png)
- Подбор параметров включает 54 классические конфигурации с тремя фолдами и восемь экспериментов с сетью. [Кросс-валидация](results/classical_cv.csv) · [Эксперименты с сетью](results/neural_experiments.csv)
- Для каждого из девяти классификаторов доступны пять тестовых метрик, оценки гамма-сигнала и предсказанные классы. Номер `source_row` соответствует строке в исходном файле, считая с единицы. [Метрики](results/test_metrics.csv) · [Прогнозы](results/test_predictions.csv)
- Журналы TensorBoard включают потери и Accuracy на обучении и валидации по каждой эпохе. [Журналы](logs/tensorboard/report) · [Значения по эпохам](results/tensorboard_history.csv)

## Запуск

Команды выполняются из корня репозитория с Python 3.12.

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m jupyter lab lab2.ipynb
```

В Windows PowerShell окружение активируется командой `.\.venv\Scripts\Activate.ps1`. В JupyterLab команда **Run All Cells** последовательно выполнит лабораторную. Исходная таблица уже включена в `data/`.

Для выполнения из терминала и обновления HTML используются две команды.

```bash
jupyter nbconvert --to notebook --execute --inplace --ExecutePreprocessor.timeout=900 lab2.ipynb
jupyter nbconvert --to html --template classic --output report.html lab2.ipynb
```

В облачной среде с домашним каталогом только для чтения перед запуском Jupyter можно задать локальные каталоги.

```bash
export JUPYTER_CONFIG_DIR="$PWD/.cache/jupyter-config" JUPYTER_DATA_DIR="$PWD/.cache/jupyter-data"
export JUPYTER_RUNTIME_DIR="$PWD/.cache/jupyter-runtime" IPYTHONDIR="$PWD/.cache/ipython"
mkdir -p "$JUPYTER_CONFIG_DIR" "$JUPYTER_DATA_DIR" "$JUPYTER_RUNTIME_DIR" "$IPYTHONDIR"
```

Сохранённые кривые можно открыть из активированного окружения.

```bash
tensorboard --logdir logs/tensorboard/report
```

Откройте `http://localhost:6006` и выберите **Time Series**. У каждого эксперимента есть ряды `train` и `validation` с метриками `epoch_loss` и `epoch_accuracy`. При новом выполнении ноутбук печатает путь к отдельному каталогу журналов.
