[[00_natasha_project]]

# Razdel — сегментация русскоязычного текста на токены и предложения

[Python-библиотека Razdel](https://github.com/natasha/razdel) — часть [проекта Natasha](https://github.com/natasha), делит русскоязычный текст на токены и предложения.

```
from razdel import tokenize, sentenize

text = 'Кружка-термос на 0.5л (50/64 см³, 516;...)'
tokens = list(tokenize(text))
tokens
[Substring(start=0, stop=13, text='Кружка-термос'),
 Substring(start=14, stop=16, text='на'),
 Substring(start=17, stop=20, text='0.5'),
 Substring(start=20, stop=21, text='л'),
 Substring(start=22, stop=23, text='(')
 ...]

text = '''
... - "Так в чем же дело?" - "Не ра-ду-ют".
... И т. д. и т. п. В общем, вся газета
... '''
list(sentenize(text))
[Substring(start=1, stop=23, text='- "Так в чем же дело?"'),
 Substring(start=24, stop=40, text='- "Не ра-ду-ют".'),
 Substring(start=41, stop=56, text='И т. д. и т. п.'),
 Substring(start=57, stop=76, text='В общем, вся газета')]
```
Скорость и качество сопоставимы или выше, чем у других открытых решений.

## Ограничения

Правила в Razdel оптимизированы для аккуратно написанных текстов с правильной пунктуацией. Решение хорошо работает с новостными статьями, художественными текстами. На постах из социальных сетей, расшифровках телефонных разговоров качество ниже.

Если между предложениями нет пробела или в конце нет точки или предложение начинается с маленькой буквы, Razdel сделает ошибку.