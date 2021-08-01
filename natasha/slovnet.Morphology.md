[[slovnet]]

# Morphology

Идет обрабатка токенизированного текста. Чтобы разделить входные данные на предложения и токены, используются [[razdel]].

```
from razdel import sentenize, tokenize
from navec import Navec
from slovnet import Morph

chunk = []
for sent in sentenize(text):
     tokens = [_.text for _ in tokenize(sent.text)]
     chunk.append(tokens)
chunk[:1]
[['Европейский', 'союз', 'добавил', 'в', 'санкционный', 'список', 'девять', 'политических', 'деятелей', 'из', 'самопровозглашенных', 'республик', 'Донбасса', '—', 'Донецкой', 'народной', 'республики', '(', 'ДНР', ')', 'и', 'Луганской', 'народной', 'республики', '(', 'ЛНР', ')', '—', 'в', 'связи', 'с', 'прошедшими', 'там', 'выборами', '.']]

navec = Navec.load('navec_news_v1_1B_250K_300d_100q.tar')
morph = Morph.load('slovnet_morph_news_v1.tar', batch_size=4)
morph.navec(navec)

markup = next(morph.map(chunk))
for token in markup.tokens:
    print(f'{token.text:>20} {token.tag}')
         Европейский ADJ|Case=Nom|Degree=Pos|Gender=Masc|Number=Sing
                союз NOUN|Animacy=Inan|Case=Nom|Gender=Masc|Number=Sing
             добавил VERB|Aspect=Perf|Gender=Masc|Mood=Ind|Number=Sing|Tense=Past|
			 		 VerbForm=Fin|Voice=Act
                   в ADP
         санкционный ADJ|Animacy=Inan|Case=Acc|Degree=Pos|Gender=Masc|Number=Sing
              список NOUN|Animacy=Inan|Case=Acc|Gender=Masc|Number=Sing
              девять NUM|Case=Nom
        политических ADJ|Case=Gen|Degree=Pos|Number=Plur
            деятелей NOUN|Animacy=Anim|Case=Gen|Gender=Masc|Number=Plur
                  из ADP
 самопровозглашенных ADJ|Case=Gen|Degree=Pos|Number=Plur
           республик NOUN|Animacy=Inan|Case=Gen|Gender=Fem|Number=Plur
            Донбасса PROPN|Animacy=Inan|Case=Gen|Gender=Masc|Number=Sing
                   — PUNCT
            Донецкой ADJ|Case=Gen|Degree=Pos|Gender=Fem|Number=Sing
            народной ADJ|Case=Gen|Degree=Pos|Gender=Fem|Number=Sing
          республики NOUN|Animacy=Inan|Case=Gen|Gender=Fem|Number=Sing
                   ( PUNCT
                 ДНР PROPN|Animacy=Inan|Case=Gen|Gender=Fem|Number=Sing
                   ) PUNCT
                   и CCONJ
           Луганской ADJ|Case=Gen|Degree=Pos|Gender=Fem|Number=Sing
            народной ADJ|Case=Gen|Degree=Pos|Gender=Fem|Number=Sing
          республики NOUN|Animacy=Inan|Case=Gen|Gender=Fem|Number=Sing
                   ( PUNCT
                 ЛНР PROPN|Animacy=Inan|Case=Gen|Gender=Fem|Number=Sing
                   ) PUNCT
                   — PUNCT
                   в ADP
               связи NOUN|Animacy=Inan|Case=Loc|Gender=Fem|Number=Sing
                   с ADP
          прошедшими VERB|Aspect=Perf|Case=Ins|Number=Plur|Tense=Past|VerbForm=Part|Voice=Act
                 там ADV|Degree=Pos
            выборами NOUN|Animacy=Inan|Case=Ins|Gender=Masc|Number=Plur
                   . PUNCT
```