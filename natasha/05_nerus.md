[[00_natasha_project]]
[[03.1_slovnet.NER]]
[[03.2_slovnet.Morphology]]
[[03.3_slovnet.Syntax]]

# Nerus — большой синтетический русскоязычный датасет с разметкой морфологии, синтаксиса и именованных сущностей

В Natasha анализ морфологии, синтаксиса и извлечение именованных сущностей делают 3 компактные модели: [Slovnet NER](https://github.com/natasha/slovnet#ner), [Slovnet Morph](https://github.com/natasha/slovnet#morphology) и [Slovnet Syntax](https://github.com/natasha/slovnet#syntax). [Качество решений](https://github.com/natasha/slovnet#evaluation) на 1–5 процентных пунктов хуже, чем у тяжёлых аналогов c BERT-архитектурой, размер в 50-75 раз меньше, скорость на CPU в 2 раза больше. Модели обучены на огромном синтетическом [датасете Nerus](https://github.com/natasha/nerus), в архиве 700 000 новостных статей с [CoNLL-U](https://universaldependencies.org/format.html)-разметкой морфологии, синтаксиса и именованных сущностей.

Slovnet NER, Morph, Syntax — примитивные модели. Когда в обучающей выборке 1000 примеров, Slovnet NER отстаёт от тяжёлого BERT-аналога на 11 процентных пунктов, когда примеров 10 000 — на 3 пункта, когда 500 000 — на 1.

Nerus — результат работы, тяжёлых моделей с BERT-архитектурой: [Slovnet BERT NER](https://github.com/natasha/slovnet/blob/master/scripts/02_bert_ner/main.ipynb), [Slovnet BERT Morph](https://github.com/natasha/slovnet/blob/master/scripts/03_bert_morph/main.ipynb), [Slovnet BERT Syntax](https://github.com/natasha/slovnet/blob/master/scripts/04_bert_syntax/main.ipynb). Обработка 700 000 новостных статей занимает 20 часов на Tesla V100. Готовый архив в открытом доступе: https://storage.yandexcloud.net/natasha-nerus/data/nerus_lenta.conllu.gz (2Gb)

У синтетической разметки высокое качество: точность определения морфологических тегов — 98%, синтаксических связей — 96%. Для NER оценки F1 по токенам: PER — 99%, LOC — 98%, ORG — 97%.

Python-пакет Nerus организует удобный интерфейс для загрузки и визуализации разметки:
```
from nerus import load_nerus

docs = load_nerus('nerus_lenta.conllu.gz')
doc = next(docs)
doc

	NerusDoc(
    id='0',
    sents=[NerusSent(
         id='0_0',
         text='Вице-премьер по социальным вопросам Татьяна Голикова рассказала, в каких регионах России ...
         tokens=[NerusToken(
              id='1',
              text='Вице-премьер',
              pos='NOUN',
              feats={'Animacy': 'Anim',
               'Case': 'Nom',
               'Gender': 'Masc',
               'Number': 'Sing'},
              head_id='7',
              rel='nsubj',
              tag='O'
          ),
          NerusToken(
              id='2',
              text='по',
              pos='ADP',
...

doc.ner.print()
Вице-премьер по социальным вопросам Татьяна Голикова рассказала, в каких регионах России зафиксирована наиболее 
                                    PER─────────────                              LOC───                     
высокая смертность от  рака, сообщает РИА Новости. По словам Голиковой, чаще всего онкологические заболевания
                                      ORG────────            PER──────             
...


sent = doc.sents[0]
sent.morph.print()
        Вице-премьер  NOUN|Animacy=Anim|Case=Nom|Gender=Masc|Number=Sing
                  по  ADP
          социальным  ADJ|Case=Dat|Degree=Pos|Number=Plur
            вопросам  NOUN|Animacy=Inan|Case=Dat|Gender=Masc|Number=Plur
             Татьяна  PROPN|Animacy=Anim|Case=Nom|Gender=Fem|Number=Sing
            Голикова  PROPN|Animacy=Anim|Case=Nom|Gender=Fem|Number=Sing
          рассказала  VERB|Aspect=Perf|Gender=Fem|Mood=Ind|Number=Sing
...
```