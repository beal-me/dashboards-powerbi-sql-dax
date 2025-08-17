# dashboard-1-dax

Este archivo contiene las **medidas DAX** utilizadas en el Dashboard 1 construido a partir de la base de datos **Sakila**.  
Las medidas permiten agregar lógica al modelo y obtener cálculos dinámicos que se actualizan en función de la selección de filtros en Power BI.

---

```dax
-- 1. Medalla Top3
-- Asigna un emoji de medalla a las 3 categorías con mayor recaudación total.
Medalla Top3 = 
VAR Ranking =
    RANKX(
        ALLSELECTED('sakila sales_by_film_category'[Categoria]),
        CALCULATE(SUM('sakila sales_by_film_category'[Total_recaudado])),
        ,
        DESC,
        DENSE
    )
RETURN
SWITCH(
    TRUE(),
    Ranking = 1, "🥇",
    Ranking = 2, "🥈",
    Ranking = 3, "🥉",
    BLANK()
)

-- 2. Promedio alquileres
-- Calcula el promedio de alquileres por película, relacionando la tabla de films con la de rentals.
Promedio alquileres = 
AVERAGEX(
    RELATEDTABLE('sakila film'),
    CALCULATE(
        COUNTROWS('sakila rental')
    )
)
