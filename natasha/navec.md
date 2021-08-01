[[natasha_project]]
# Navec — компактные эмбеддинги для русского языка

[Библиотека Navec](https://github.com/natasha/navec) — часть [проекта Natasha](https://github.com/natasha), коллекция предобученных эмбеддингов для русского языка. Качество решения сравнимо или выше, чем у статических моделей от RusVectores, размер в 5–6 раз меньше.

## Принцип работы

`hudlit_12B_500K_300d_100q` — [GloVe-эмбеддинги](https://nlp.stanford.edu/projects/glove/) обученные на [145ГБ художественной литературы](https://github.com/natasha/corus#load_librusec). Архив с текстами взяты из [проекта RUSSE](https://russe.nlpub.org/downloads/). Используется[оригинальная реализация GloVe на C](https://github.com/stanfordnlp/GloVe), обернутая в [удобный Python-интерфейс](https://github.com/natasha/navec/blob/master/navec/train/glove.py).

Эксперименты на большом датасете быстрее с GloVe, чем word2vec. Один раз считаеся матрица коллокаций, по ней готовятся эмбеддинги разных размерностей, выбирается оптимальный вариант (оригинальная статья про GloVe: [Global Vectors for Word Representation](https://nlp.stanford.edu/pubs/glove.pdf))

Natasha работает с текстами новостей. В них мало опечаток, проблему OOV-токенов решает большой словарь. 250 000 строк в таблице `news_1B_250K_300d_100q` покрывают 98% слов в новостных статьях.

Размер словаря `hudlit_12B_500K_300d_100q` — 500 000 записей, он покрывает 98% слов в художественных текстах. Оптимальная размерность векторов — 300. Таблица 500 000 × 300 из float-чисел занимает 578МБ, размер архива с весами `hudlit_12B_500K_300d_100q` в 12 раз меньше (48МБ) засчет квантизации.

Квантованные эмбеддинги проигрывают обычным по скорости. Сжатый вектор перед использованием проходит этап распаковки. Реализация процедуры, [с помощью Numpy](https://github.com/natasha/navec/blob/master/navec/pq.py#L40-L43), в PyTorch [используется `torch.gather`](https://github.com/natasha/slovnet/blob/master/slovnet/model/emb.py#L29-L39). В Slovnet NER доступ к таблице эмбеддингов занимает 0.1% от общего времени вычислений.

## Использование

[Список предобученных моделей](https://github.com/natasha/navec#downloads), [инструкция по установке](https://github.com/natasha/navec#installation) — в [репозитории Navec](https://github.com/natasha/navec).

Модуль `NavecEmbedding` из [библиотеки Slovnet](https://github.com/natasha/slovnet) интегрирует Navec в PyTorch-модели:

```
import torch

from navec import Navec
from slovnet.model.emb import NavecEmbedding

path = 'hudlit_12B_500K_300d_100q.tar'  # 51MB
navec = Navec.load(path)  # ~1 sec, ~100MB RAM

words = ['навек', '<unk>', '<pad>']
ids = [navec.vocab[_] for _ in words]

emb = NavecEmbedding(navec)
input = torch.tensor(ids)

emb(input)  # 3 x 300
tensor([[ 4.2000e-01,  3.6666e-01,  1.7728e-01,
        [ 1.6954e-01, -4.6063e-01,  5.4519e-01,
        [ 0.0000e+00,  0.0000e+00,  0.0000e+00,
	  ...
```