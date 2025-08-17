# Dashboard 1 – SQL

Este documento reúne algunas consultas utilizadas como borrador para explorar y validar información en la base de datos **Sakila**.  
Se incluyen cálculos de recaudación, cantidad de alquileres, conteos, promedios y pruebas con CTE.

---
```sql
-- 1. Recaudación por categoría
SELECT
	category.name AS categoria,
    SUM(payment.amount) AS total_recaudado
FROM category
LEFT JOIN film_category ON category.category_id=film_category.category_id
LEFT JOIN film ON film_category.film_id=film.film_id
LEFT JOIN inventory ON film.film_id=inventory.film_id
LEFT JOIN rental ON inventory.inventory_id=rental.inventory_id
LEFT JOIN payment ON rental.rental_id=payment.rental_id
GROUP BY category.name
ORDER BY total_recaudado DESC;

-- 2. Cantidad de alquileres por categoría
SELECT
	category.name AS categoria,
    COUNT(rental.rental_id) AS alquileres
FROM category
LEFT JOIN film_category ON category.category_id=film_category.category_id
LEFT JOIN film ON film_category.film_id=film.film_id
LEFT JOIN inventory ON film.film_id=inventory.film_id
LEFT JOIN rental ON inventory.inventory_id=rental.inventory_id
LEFT JOIN payment ON rental.rental_id=payment.rental_id
GROUP BY category.name
ORDER BY alquileres DESC;

-- 3. Total de alquileres en la base
SELECT
	COUNT(rental.rental_id)
FROM rental;

-- 4. Promedio de rental_id por categoría
SELECT
	category.name AS categoria,
    AVG(rental.rental_id)
FROM category
LEFT JOIN film_category ON category.category_id=film_category.category_id
LEFT JOIN film ON film_category.film_id=film.film_id
LEFT JOIN inventory ON film.film_id=inventory.film_id
LEFT JOIN rental ON inventory.inventory_id=rental.inventory_id
LEFT JOIN payment ON rental.rental_id=payment.rental_id
GROUP BY category.name;

-- 5. Películas distintas en el catálogo
SELECT DISTINCT
	film.title
FROM film;

-- 6. CTE – Alquileres promedio por categoría
WITH alq_por_pelicula AS (
	SELECT
		film.film_id,
		COUNT(rental.rental_id) AS total_alquileres
	FROM film
	LEFT JOIN inventory ON film.film_id=inventory.film_id
	LEFT JOIN rental ON inventory.inventory_id=rental.inventory_id
	GROUP BY film.film_id
),
categoria_alq AS (
	SELECT
		film.film_id,
        category.name AS categoria
	FROM film
	JOIN film_category ON film.film_id=film_category.film_id
	JOIN category ON film_category.category_id=category.category_id
	GROUP BY film.film_id, category.name
)
SELECT
	categoria_alq.categoria AS categoria,
    AVG(alq_por_pelicula.total_alquileres) AS promedio_alq
FROM categoria_alq
JOIN alq_por_pelicula ON categoria_alq.film_id=alq_por_pelicula.film_id
GROUP BY categoria_alq.categoria
ORDER BY promedio_alq DESC;

-- 7. Alquileres por categoría (orden alfabético)
SELECT
	category.name AS categoria,
	COUNT(rental.rental_id) AS cant_alquileres
FROM category
LEFT JOIN film_category ON category.category_id=film_category.category_id
LEFT JOIN film ON film_category.film_id=film.film_id
LEFT JOIN inventory ON film.film_id=inventory.film_id
LEFT JOIN rental ON inventory.inventory_id=rental.inventory_id
GROUP BY category.name
ORDER BY category.name;

-- 8. Cantidad de películas por categoría
SELECT
	COUNT(film.film_id) AS cant_peliculas,
    category.name AS categoria
FROM film
LEFT JOIN film_category ON film.film_id=film_category.film_id
LEFT JOIN category ON film_category.category_id=category.category_id
GROUP BY category.name
ORDER BY category.name;

-- 9. Películas sin alquileres
SELECT
	category.name,
	film.film_id,
    rental.rental_id
FROM category
LEFT JOIN film_category ON category.category_id=film_category.category_id
LEFT JOIN film ON film_category.film_id=film.film_id
LEFT JOIN inventory ON film.film_id=inventory.film_id
LEFT JOIN rental ON inventory.inventory_id=rental.inventory_id
WHERE rental_id IS NULL
GROUP BY film.film_id, rental.rental_id, category.name;
