---
## Front matter
lang: ru-RU
title: Лабораторная работа №5
subtitle: "Модель хищник-жертва"
author:
  - Чемоданова Ангелина Александровна
teacher:
  - Кулябов Д. С.
  - д.ф.-м.н., профессор
  - профессор кафедры теории вероятностей и кибербезопасности 
institute:
  - Российский университет дружбы народов имени Патриса Лумумбы, Москва, Россия
date: 19 апреля 2025

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

## Докладчик

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

  * Чемоданова Ангелина Александровна
  * Cтудентка НФИбд02-22
  * Российский университет дружбы народов имени Патриса Лумумбы
  * [1132226443@pfur.ru](mailto:1132226443@pfur.ru)
  * <https://github.com/aachemodanova>

:::
::: {.column width="30%"}

![](./image/angelina.jpg)

:::
::::::::::::::

## Цель работы

Исследовать математическую модель хищник-жертва.

## Задание

Для модели «хищник-жертва»:

$$
\begin{cases}
\dfrac{dx}{dt} = -0.63 x(t)+0.019 x(t)y(t)\\
\dfrac{dy}{dt} = 0.59y(t)- 0.018 x(t)y(t)
\end{cases}
$$

Постройте график зависимости численности хищников от численности жертв, а также графики изменения численности хищников и численности жертв при следующих начальных условиях: $x_0 = 7$, $y_0 = 12$. Найдите стационарное состояние системы.

## Поиск стационарного состояния системы

$$
\begin{cases}
  -0.63 x(t)+0.019 x(t)y(t) = 0\\
  0.59y(t)- 0.018 x(t)y(t) = 0
\end{cases}
$$

$$
\begin{cases}
  -0.63 +0.019 y(t) = 0\\
  0.59- 0.018 x(t) = 0
\end{cases}
$$

## Реализация в Julia

```julia
using DifferentialEquations, Plots
# Начальные условия
u0 = [7, 12]
p = [-0.63, -0.019, -0.59, -0.018]
tspan = (0.0, 50.0)
```

## Реализация в Julia

```julia
# система ДУ, описывающей модель Лотки-Вольтерры
function LV(u, p, t)
    x, y = u
    a, b, c, d = p
    dx = a*x - b*x*y
    dy = -c*y + d*x*y
    return [dx, dy]
end

prob = ODEProblem(LV, u0, tspan, p)
sol = solve(prob, Tsit5())
```

## Реализация в Julia

```julia
plot(sol, title = "Модель Лотки-Вольтерры", 
    xaxis = "Время", yaxis = "Численность популяции", 
    label = ["жертвы" "хищники"])

plot(sol, vars=(1, 2), label="y от x", 
    xlabel="x, жертвы", ylabel="y, хищники", 
    title="Фазовый портрет")
```

## Реализация в Julia

![Решение модели при $x_0 = 7, \, y_0=12$. Julia](image/2.PNG){#fig:001 width=70%}

## Реализация в Julia

![Фазовый портрет модели при $x_0 = 7, \, y_0=12$. Julia](image/1.PNG){#fig:002 width=70%}

## Реализация в Julia

```julia
# проверка стационарной точки
x_c = p[3]/p[4]
y_c = p[1]/p[2]
u0_c = [x_c, y_c]
prob2 = ODEProblem(LV, u0_c, tspan, p)
sol2 = solve(prob2, Tsit5())
```

## Реализация в Julia

```julia
plot(sol2, xaxis = "Время", 
    yaxis = "Численность популяции", 
    label = ["Жертвы" "Хищники"])
```

## Реализация в Julia

![Решение модели при $x_0 = x_c, \, y_0=y_c$. Julia](image/3.PNG){#fig:003 width=70%}

## Реализация в OpenModelica

```Modelica
model lab5_1

parameter Real a=-0.63;
parameter Real b=-0.019;
parameter Real c=-0.59;
parameter Real d=-0.018;

parameter Real x0=7;
parameter Real y0=12;

Real x(start=x0);
Real y(start=y0);

equation

der(x) = a*x - b*x*y;
der(y) = -c*y + d*x*y;

end lab5_1;
```

## Реализация в OpenModelica

![Решение модели при $x_0 = 7, \, y_0=12$. OpenModelica](image/5.jpg){#fig:004 width=70%}

## Реализация в OpenModelica

![Фазовый портрет модели при $x_0 = 7, \, y_0=12$. OpenModelica](image/4.jpg){#fig:005 width=70%}

## Реализация в OpenModelica

```Modelica
model lab5_2
parameter Real a=-0.63;
parameter Real b=-0.019;
parameter Real c=-0.59;
parameter Real d=-0.018;

parameter Real x0=c/d;
parameter Real y0=a/b;

Real x(start=x0);
Real y(start=y0);

equation

der(x) = a*x - b*x*y;
der(y) = -c*y + d*x*y;
end lab5_2;
```

## Реализация в OpenModelica

![Решение модели при $x_0 = x_c, \, y_0=y_c$. OpenModelica](image/6.jpg){#fig:006 width=70%}

## Выводы

Построили математическую модель хищник жертва и провели анализ.
