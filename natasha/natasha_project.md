# Natasha

Natasha — решает базовые задачи обработки естественного русского языка: сегментация на токены и предложения, морфологический и синтаксический анализ, лемматизация, извлечение, нормализация именованных сущностей.

 Для новостных статей качество на всех задачах [сравнимо или превосходит существующие решения](https://github.com/natasha/natasha#evaluation). Библиотека поддерживает Python 3.5+ и PyPy3, не требует GPU, зависит только от NumPy.

В 2020 году в проекте Natasha удалось вплотную приблизится по качеству к DeepPavlov BERT NER, размер модели получился в 75 раз меньше (27МБ), потребление памяти в 30 раз меньше (205МБ), скорость в 2 раза больше на CPU (25 статей в секунду).

Natasha ([Slovnet NER](https://github.com/natasha/slovnet#ner)) = [Slovnet BERT NER](https://github.com/natasha/slovnet/blob/master/scripts/02_bert_ner/main.ipynb) — аналог DeepPavlov BERT NER + дистилляция через синтетическую разметку ([Nerus](https://github.com/natasha/nerus)) в WordCNN-CRF c квантованными эмбеддингами ([Navec](https://github.com/natasha/navec)) + движок для инференса на NumPy.

Natasha объединяет под одним интерфейсом другие библиотеки проекта. Natasha хороша для демонстрации технологий. В работе стоит использовать низкоуровневые библиотеки напрямую:

-   [[razdel]] (https://github.com/natasha/razdel) — сегментация текста на предложения и токены;
-   [[navec]] (https://github.com/natasha/navec) — качественные компактные эмбеддинги;
-   [[slovnet]] (https://github.com/natasha/slovnet) — современные компактные модели для морфологии, синтаксиса, NER;
-   [Yargy](https://github.com/natasha/yargy) — правила и словари для извлечения структурированной информации;
-   [[ipymarkup]] (https://github.com/natasha/ipymarkup) — визуализация NER и синтаксической разметки;
-   [Corus](https://github.com/natasha/corus) — коллекция ссылок на публичные русскоязычные датасеты;
-   [[nerus]] (https://github.com/natasha/nerus) — большой корпус с автоматической разметкой именованных сущностей, морфологии и синтаксиса.

Natasha извлекает стандартные сущности: имена (PER), названия топонимов (LOC) и организаций (ORG). Решение показывает хорошее качество на новостях. 
Для работы с другими сущностями и типами текстов нужно обучить новую модель.  Компактный размер и скорость работы =  сложность подготовки модели. [Скрипт-ноутбук для подготовки тяжёлой модели учителя](https://github.com/natasha/slovnet/blob/master/scripts/02_bert_ner/main.ipynb), [скрипт-ноутбук для модели-ученика](https://github.com/natasha/slovnet/blob/master/scripts/05_ner/main.ipynb), [инструкции по подготовке квантованных эмбеддингов](https://github.com/natasha/navec#development).
