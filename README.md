# Two-actors-who-cast-together-the-most
## SQL challenge for ironhack TA


### La siguiente query resuelve la duda que tenemos que cuales son los actores que participan en más películas juntos. Para el ejercicio se necesitó el uso de subqueries enfocadas en saber el id de los actores, posteriormente esta subquery se une a la master query donde obtendremos el nombre de los actores y de las películas.

#### En lo personal me dejo mucho aprendizaje este ejercicio, ya que nunca había hecho algo similar, y con un poco de investigación logré resolver el problema.

SELECT
(SELECT CONCAT(first_name,' ',last_name) FROM actor WHERE actor_id = C.first_actor) AS first_actor,
(SELECT CONCAT(first_name,' ',last_name) FROM actor WHERE actor_id = C.second_actor) AS second_actor,
F.title
FROM
(SELECT
A.actor_id AS first_actor,
B.actor_id AS second_actor
FROM
film_actor A
JOIN film_actor B ON (A.film_id = B.film_id)
WHERE A.actor_id <> B.actor_id
GROUP BY 1,2
ORDER BY COUNT(A.film_id) DESC
LIMIT 1) C
JOIN film_actor D ON C.first_actor = D.actor_id
JOIN film_actor E ON C.second_actor = E.actor_id
JOIN film F ON F.film_id = D.film_id AND F.film_id = E.film_id
