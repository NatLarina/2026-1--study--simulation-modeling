# Отчёт по лабораторной работе: агентное моделирование Daisyworld

## Титульный лист

ФИО студента: Ларина Наталья Денисовна  
Группа: НФИбд-01-23  
№ студенчекского: 1132236025  

---

Дата сдачи: 21 марта 2026 г.  

---

Москва 2025

<div style="page-break-after: always;"></div>

## 1. Теоретическая справка

### 1.1 Агентное моделирование
Агентное моделирование (Agent‑Based Modeling, ABM) — метод исследования сложных систем, в котором глобальное поведение возникает из взаимодействий множества автономных агентов. В отличие от традиционных дифференциальных уравнений, ABM позволяет учитывать гетерогенность, локальные взаимодействия и адаптивное поведение. Основные компоненты: агенты (обладают свойствами и правилами), среда (пространство, ресурсы), взаимодействия (локальные или глобальные). Ключевой принцип — эмерджентность: коллективные паттерны не задаются явно, а появляются снизу вверх.

### 1.2 Модель Daisyworld
Модель Daisyworld была предложена Джеймсом Лавлоком и Эндрю Уотсоном в 1983 году для иллюстрации гипотезы Геи. В модели на поверхности планеты растут два вида маргариток: чёрные (низкое альбедо, поглощают тепло) и белые (высокое альбедо, отражают тепло). Температура каждой клетки зависит от солнечной светимости и альбедо покрытия. Маргаритки могут размножаться только в благоприятном температурном диапазоне. Взаимодействие живых организмов и среды приводит к саморегуляции: при повышении светимости увеличивается доля белых маргариток, что охлаждает планету, и наоборот. Система остаётся в равновесии в широком диапазоне внешних воздействий.

### 1.3 Реализация модели
В работе используется пакет Agents.jl (язык Julia). Пространство — двумерная сетка 30×30 с периодическими границами. Агенты (`Daisy`) имеют тип (breed: `:white` или `:black`), возраст и альбедо. Локальная температура рассчитывается на основе поглощённой энергии, затем происходит диффузия тепла. Маргаритки стареют, погибают при превышении `max_age` и размножаются в пустые соседние клетки с вероятностью, зависящей от температуры. В экспериментах варьируются начальное покрытие белыми маргаритками (`init_white`) и максимальный возраст (`max_age`), а также исследуется сценарий изменения солнечной светимости (`scenario = :ramp`).

---

## 2. Результаты экспериментов

### 2.1 Базовая визуализация (п. 3.2.4)

На начальном этапе (шаг 0) модель инициализируется случайным расположением 20% чёрных и 20% белых маргариток. Температурное поле (цвет фона) практически однородно.

![](daisy_step001.png)

Через 5 шагов видно локальное перераспределение: чёрные маргаритки создают вокруг себя тёплые зоны, а белые — холодные. Начинается конкуренция за свободные клетки.

![](daisy_step005.png)

К шагу 40 система достигает квазистационарного состояния: чёрные и белые маргаритки сосуществуют в пропорции, поддерживающей температуру в благоприятном диапазоне.

![](daisy_step040.png)

---

### 2.2 Анимация (п. 3.2.5)

Видео `simulation.mp4` (сохранено в папке `plots`) демонстрирует динамику модели за 60 шагов. На видео хорошо видно, как маргаритки постепенно колонизируют свободные участки, а температурные аномалии сглаживаются за счёт диффузии.

---

### 2.3 Динамика численности (п. 3.2.6)

На графике ниже показано изменение количества чёрных и белых маргариток за 1000 шагов при постоянной светимости (`solar_luminosity = 1.0`). Начальное соотношение 20% на 20% постепенно смещается в сторону увеличения белых маргариток, так как они охлаждают планету, что способствует их размножению. После 400 шагов численность стабилизируется.

![](daisy_count.png)

---

### 2.4 Динамика модели при изменении светимости (п. 3.2.7)

В этом эксперименте солнечная светимость сначала увеличивается, а затем уменьшается (`scenario = :ramp`). На комплексном графике представлены:

- **Верхний график**: численность чёрных и белых маргариток. При росте светимости белые маргаритки вытесняют чёрные, охлаждая планету.
- **Средний график**: средняя температура планеты. Она остаётся почти постоянной, несмотря на изменение светимости, благодаря обратной связи через альбедо.
- **Нижний график**: изменение светимости (входной параметр).

Модель демонстрирует гомеостаз: температура колеблется в узком диапазоне, а соотношение видов адаптируется к внешним изменениям.

![](daisy_luminosity.png)

---

### 2.5 Параметрические исследования (пп. 3.2.8–3.2.10)

Для анализа влияния параметров были рассмотрены комбинации максимального возраста маргариток (`max_age = 25` и `40`) и начальной доли белых (`init_white = 0.2` и `0.8`). Ниже приведены графики, полученные для каждого набора параметров. Все изображения сохранены в папке `plots` с именами, отражающими параметры.

#### 2.5.1 Базовая визуализация (п. 3.2.8)

**max_age=25, init_white=0.2**  
| Шаг 1 | Шаг 5 | Шаг 40 |
|-------|-------|-------|
| ![](daisyworld_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.2_max_age=25_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4_step01.png) | ![](daisyworld_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.2_max_age=25_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4_step04.png) | ![](daisyworld_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.2_max_age=25_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4_step40.png) |

**max_age=25, init_white=0.8**  
| Шаг 1 | Шаг 5 | Шаг 40 |
|-------|-------|-------|
| ![](daisyworld_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.8_max_age=25_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4_step01.png) | ![](daisyworld_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.8_max_age=25_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4_step04.png) | ![](daisyworld_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.8_max_age=25_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4_step40.png) |

**max_age=40, init_white=0.2**  
| Шаг 1 | Шаг 5 | Шаг 40 |
|-------|-------|-------|
| ![](daisyworld_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.2_max_age=40_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4_step01.png) | ![](daisyworld_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.2_max_age=40_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4_step04.png) | ![](daisyworld_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.2_max_age=40_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4_step40.png) |

**max_age=40, init_white=0.8**  
| Шаг 1 | Шаг 5 | Шаг 40 |
|-------|-------|-------|
| ![](daisyworld_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.8_max_age=40_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4_step01.png) | ![](daisyworld_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.8_max_age=40_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4_step04.png) | ![](daisyworld_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.8_max_age=40_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4_step40.png) |

#### 2.5.2 Динамика численности (п. 3.2.9)

**max_age=25, init_white=0.2**  
![](daisy-count_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.2_max_age=25_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4.png)

**max_age=25, init_white=0.8**  
![](daisy-count_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.8_max_age=25_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4.png)

**max_age=40, init_white=0.2**  
![](daisy-count_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.2_max_age=40_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4.png)

**max_age=40, init_white=0.8**  
![](daisy-count_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.8_max_age=40_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4.png)

#### 2.5.3 Динамика при изменении светимости (п. 3.2.10)

**max_age=25, init_white=0.2**  
![](daisy-luminosity_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.2_max_age=25_scenario=ramp_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4.png)

**max_age=25, init_white=0.8**  
![](daisy-luminosity_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.8_max_age=25_scenario=ramp_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4.png)

**max_age=40, init_white=0.2**  
![](daisy-luminosity_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.2_max_age=40_scenario=ramp_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4.png)

**max_age=40, init_white=0.8**  
![](daisy-luminosity_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.8_max_age=40_scenario=ramp_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4.png)

---

## 3. Заключение

В ходе лабораторной работы была реализована и исследована агентная модель Daisyworld на Julia с использованием Agents.jl. Полученные результаты подтверждают основные положения гипотезы Геи:

- Локальные правила размножения маргариток приводят к глобальной саморегуляции температуры.
- Система устойчива к изменениям внешней светимости (п. 2.4).
- Параметры `max_age` и `init_white` влияют на скорость установления равновесия, но не нарушают его (п. 2.5).
- Визуализация и анимация наглядно демонстрируют эмерджентное поведение.

Все графики и видео сохранены в папке `plots`. Исходный код модели и скрипты анализа размещены в репозитории.