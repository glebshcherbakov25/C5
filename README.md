# Crypto C-5 Factor Model – **Synthetic Data (2020-2024)**

Проект демонстрирует тестирование факторной модели **C-5**  
(`MKT`, `SMB`, `MOM`, `VAL`, `NET`) для доходности Bitcoin на *синтетическом* наборе
ежемесячных наблюдений 2020 - 2024.

## Структура

├── model_C5_synth.ipynb # Python-версия


└── model_C5_synth.R # R-версия



└── factors.csv # генерируется скриптами


└── btc_synth_fit.png # сохраняется скриптами

└── README.md
## 
Результаты и выводы (синтетический прогон, seed = 42)

| Метрика | Значение |
|---------|----------|
| R² модели | **0.54** |
| MAE      | **≈ 0.028** (2.8 п.п. месячной доходности) |
| N (месяцев) | 60 (2020-01 … 2024-12) |

### Оценённые β-коэффициенты (OLS, Python)
| Фактор | Оценка | p-value | «Истинное» β |
|--------|--------|---------|--------------|
| MKT | **+0.69** | 0.000 | 0.90 |
| SMB | +0.34 | 0.061 | 0.40 |
| MOM | +0.35 | 0.203 | 0.30 |
| VAL | **+0.85** | 0.001 | 0.50 |
| NET | **+0.66** | 0.045 | 0.20 |
| const | +0.002 | 0.627 | 0 |

###  Ключевые наблюдения  

* **Качество подгонки.** Значение R² ≈ 0.54 для полностью синтетического набора данных можно считать удовлетворительным: это означает, что чуть более половины вариации доходности BTC объясняется заложенной факторной структурой.  

* **Согласованность оценок β.**  
  * Рыночный фактор (**MKT**) воспроизведён наиболее близко к заданному значению (0.69 против исходных 0.90) и статистически значим (p < 0.01).  
  * Фактор стоимости (**VAL**) оценён несколько выше исходного β (0.85 против 0.50); это иллюстрирует, что дополнительный шум и сильная обратная связь с фактором MOM могут приводить к смещению оценок.  
  * Сетевой фактор (**NET**) получен статистически значимым (p ≈ 0.045) и с более высоким значением, чем заложено (0.66 против 0.20); это объясняется случайной высокой корреляцией NET с рыночными флуктуациями в конкретной симуляции.  
  * Факторы **SMB** и **MOM** оказались статистически незначимыми (p ≈ 0.06 и p ≈ 0.20 соответственно); их сигнал оказывается легко размытым в небольших выборках при наличии шума.  

* **Альфа (const)** статистически не отличается от нуля, что свидетельствует о корректно заданной спецификации: модель, в среднем, не оставляет систематической переоценки или недооценки доходности BTC.  

* **Диагностика остатков** подтверждает адекватность модели: тесты Жарка-Бера и Omnibus не выявляют серьёзных отклонений от нормальности распределения ошибок, а значение критерия Дарбина–Уотсона близко к 2, что указывает на отсутствие автокорреляции остатков.  

* **Практический итог:** даже в искусственно сгенерированной среде модель C-5 улавливает основную динамику доходностей BTC; наибольший вклад в вариацию дают рыночный и value-факторы. Размерной и сетевой эффекты проявляются сильнее при наличии существенного «шока» коэффициентов β или увеличении объёма выборки.



---
