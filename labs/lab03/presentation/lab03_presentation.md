---
presentation:
  theme: white.css
  transition: slide
  width: 1200
  height: 800
  center: true
  enableSpeakerNotes: true
---

## Титульный слайд

**Лабораторная работа:** агентное моделирование Daisyworld  
**Студент:** Ларина Наталья Денисовна  
**Группа:** НФИбд-01-23  
**№ студенческого:** 1132236025  


<!-- slide -->

## 1. Теоретическая справка

### 1.1 Агентное моделирование
- **ABM** — метод исследования сложных систем  
- Глобальное поведение возникает из взаимодействий автономных агентов  
- Компоненты: агенты, среда, взаимодействия  
- Ключевой принцип: **эмерджентность**

### 1.2 Модель Daisyworld
- Предложена Джеймсом Лавлоком и Эндрю Уотсоном (1983)  
- Два вида маргариток: чёрные (низкое альбедо) и белые (высокое альбедо)  
- Саморегуляция температуры через обратную связь альбедо  

<!-- slide -->

## Реализация модели (Agents.jl, Julia)

- Пространство: 30×30, периодические границы  
- Агенты: `Daisy` (тип, возраст, альбедо)  
- Динамика:  
  - Расчёт локальной температуры  
  - Диффузия тепла  
  - Старение и смертность  
  - Размножение в соседние пустые клетки  
- Параметры: `max_age`, `init_white`, сценарий светимости (`:ramp`)

<!-- slide -->

## 2. Результаты экспериментов

### 2.1 Базовая визуализация (п. 3.2.4)

**Шаг 0** – начальное состояние  
![](daisy_step001.png)

**Шаг 5** – начало конкуренции  
![](daisy_step005.png)

**Шаг 40** – квазистационарное состояние  
![](daisy_step040.png)

<!-- slide -->

## 2.2 Анимация (п. 3.2.5)

- Видео `simulation.mp4` (60 шагов)  
- Наглядно видна колонизация пространства и температурная динамика

<!-- slide -->

## 2.3 Динамика численности (п. 3.2.6)

- 1000 шагов, постоянная светимость  
- Установление равновесия после 400 шагов  

![](daisy_count.png)

<!-- slide -->

## 2.4 Динамика при изменении светимости (п. 3.2.7)

- Сценарий `:ramp` (увеличение, затем уменьшение светимости)  
- Температура остаётся почти постоянной  
- Соотношение видов адаптируется к внешним изменениям  

![](daisy_luminosity.png)

<!-- slide -->

## 2.5 Параметрические исследования (пп. 3.2.8–3.2.10)

Варьируемые параметры:  
- `max_age` = 25, 40  
- `init_white` = 0.2, 0.8  

Для каждой комбинации построены:
- Визуализация на шагах 1, 5, 40  
- Графики численности  
- Графики динамики при изменении светимости  

<!-- slide -->

### 2.5.1 Базовая визуализация (max_age=25, init_white=0.2)

| Шаг 1 | Шаг 5 | Шаг 40 |
|-------|-------|-------|
| ![](daisyworld_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.2_max_age=25_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4_step01.png) | ![](daisyworld_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.2_max_age=25_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4_step04.png) | ![](daisyworld_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.2_max_age=25_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4_step40.png) |

**Вывод:** быстрое установление равновесия, доминирование белых.

<!-- slide -->

### 2.5.2 Динамика численности (все комбинации)

**max_age=25, init_white=0.2**  
![](daisy-count_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.2_max_age=25_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4.png)

**max_age=25, init_white=0.8**  
![](daisy-count_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.8_max_age=25_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4.png)

<!-- slide -->

**max_age=40, init_white=0.2**  
![](daisy-count_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.2_max_age=40_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4.png)

**max_age=40, init_white=0.8**  
![](daisy-count_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.8_max_age=40_scenario=default_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4.png)

<!-- slide -->

### 2.5.3 Динамика при изменении светимости (все комбинации)

**max_age=25, init_white=0.2**  
![](daisy-luminosity_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.2_max_age=25_scenario=ramp_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4.png)

**max_age=25, init_white=0.8**  
![](daisy-luminosity_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.8_max_age=25_scenario=ramp_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4.png)

<!-- slide -->

**max_age=40, init_white=0.2**  
![](daisy-luminosity_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.2_max_age=40_scenario=ramp_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4.png)

**max_age=40, init_white=0.8**  
![](daisy-luminosity_albedo_black=0.25_albedo_white=0.75_init_black=0.2_init_white=0.8_max_age=40_scenario=ramp_seed=165_solar_change=0.005_solar_luminosity=1.0_surface_albedo=0.4.png)

<!-- slide -->

## 3. Заключение

- Модель демонстрирует **эмерджентную саморегуляцию** температуры  
- Система устойчива к изменениям светимости (гомеостаз)  
- Параметры `max_age` и `init_white` влияют на переходную динамику, но не нарушают равновесие  
- Полученные графики и видео подтверждают гипотезу Геи  

**Исходный код и все материалы** доступны в репозитории.