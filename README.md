# Компьютерное зрение — лабораторные (Jupyter)

В репозитории хранится актуальная версия лабораторной работы №1.

## Актуальный ноутбук

- `notebooks/lab1_dvm_car_color_classification.ipynb`

## Тема ЛР1

**Классификация цвета автомобиля** на датасете DVM (фронтальные виды).

По заданию в ноутбуке реализовано:
1. Классификатор, написанный своими руками (`ScratchResNet`) и обучение с нуля.
2. Два предобученных классификатора с fine-tuning:
   - `ResNet18 (ImageNet)`
   - `MobileNetV3-Large (ImageNet)`
3. Оценка качества по `F1_macro` и проверка условия `F1_macro > 0.8`.
4. Сравнение всех трех моделей и автоматический блок итоговых выводов.

## Подготовка данных

Источник: https://deepvisualmarketing.github.io/  
Скачать архив **Quality checked front-view images (730 MB)** и распаковать в:

```text
data/raw/dvm_front/
```

Ожидаемая структура:

```text
data/raw/dvm_front/Brand/Model/Year/Color/*.jpg
```

## Быстрый запуск

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook
```

После запуска откройте `notebooks/lab1_dvm_car_color_classification.ipynb`.
