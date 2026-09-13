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
5. Логирование обучения и оценки через `tqdm` (train/val/test прогресс-бары).
6. Усиленная предобработка под дисбаланс: merge близких цветов + undersampling частых + oversampling редких.

## Подготовка данных

Источник: https://deepvisualmarketing.github.io/  
Рекомендуемый архив: **Quality checked front-view images (730 MB)** (`Confirmed_fronts.zip`).

Распакуйте архив в:

```text
data/raw/dvm_front/
```

Поддерживаются оба варианта структуры DVM:

```text
# Вариант 1 (из manual)
data/raw/dvm_front/Brand/Model/Year/Color/*.jpg

# Вариант 2 (Confirmed_fronts)
data/raw/dvm_front/Brand/Year/Brand$$Model$$Year$$Color$$...jpg
```

Если храните данные в другом месте, можно указать путь через переменную окружения:

```bash
export DVM_DATA_ROOT="/absolute/path/to/dvm_front"
```

## Быстрый запуск

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook
```

После запуска откройте `notebooks/lab1_dvm_car_color_classification.ipynb`.

### Быстрый запуск в Windows (PowerShell)

```powershell
py -3.11 -m venv .venv
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m jupyter notebook
```

## Примечания по запуску

- В ноутбуке есть отдельная ячейка **«Скачать и распаковать DVM front-view»**.
  - По умолчанию она безопасна и ничего не качает (`DOWNLOAD_DVM_FRONT = False`).
  - Чтобы скачать датасет прямо из ноутбука, поставьте `DOWNLOAD_DVM_FRONT = True` и выполните эту ячейку один раз.
- В `requirements.txt` оставлены только реально используемые зависимости ноутбука, чтобы установка на Windows не падала на сборке лишних C-пакетов.
- По умолчанию в `Config` preprocessing нацелен на диапазон **14–15 классов** после очистки:
  - удаляется только `unlisted`,
  - выполняется контролируемый merge самых редких/близких цветов до целевого диапазона,
  - затем применяется **undersampling + oversampling + WeightedRandomSampler**.
- В актуальной версии preprocessing используется более «защитимая» схема для метрики:
  - удаляется только `unlisted`,
  - похожие редкие цвета (например `turquoise`, `indigo`) маппятся в базовые по необходимости,
  - train дополнительно балансируется under+over sampling на уровне датафрейма.
- В ноутбуке устройство выбирается автоматически (`cuda`, если доступна, иначе `cpu`).
- Для ускорения на CPU можно уменьшить `image_size` и `epochs` в `Config`.
