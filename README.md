# bibliotecaSQL

Modelo conceptual
![alt text](<Modelo Conceptual.png>)

Al realizar la identificación de variables se detecta la necesidad de normalizar el genero del libro.

Modelo Fisico
![alt text](<Modelo  Físico.png>)

Listar todos los libros disponibles
```sql
# SELECT tittle FROM book;
```
Buscar libros por género
```sql
# SELECT b.tittle,g.name, FROM book AS b JOIN book_genre AS bg ON b.book_id=bg.book_id JOIN genre AS g ON bg.genre_id=g.genre_id GROUP BY genre_id;
```
Obtener información de un libro por ISBN
```sql
# SELECT tittle,edition,publication FROM book WHERE ISBN='consulta deseada';
```
Contar el número de libros en la biblioteca
```sql
# SELECT COUNT(tittle) AS total_libros FROM book;
```
Listar todos los autores
```sql
# SELECT name FROM author;
```
Buscar autores por nombre
```sql
# Introduce aquí las consultas.
```
Obtener todos los libros de un autor específico
```sql
# Introduce aquí las consultas.
```
Listar todas las ediciones de un libro
```sql
# Introduce aquí las consultas.
```
Obtener la última edición de un libro
```sql
# Introduce aquí las consultas.
```
Contar cuántas ediciones hay de un libro específico
```sql
# Introduce aquí las consultas.
```
Listar todas las transacciones de préstamo
```sql
# Introduce aquí las consultas.
```
Obtener los libros prestados actualmente
```sql
# Introduce aquí las consultas.
```
Contar el número de transacciones de un miembro específico
```sql
# Introduce aquí las consultas.
```
Listar todos los miembros de la biblioteca
```sql
# Introduce aquí las consultas.
```
Buscar un miembro por nombre:
```sql
# Introduce aquí las consultas.
```
Obtener las transacciones de un miembro específico
```sql
# Introduce aquí las consultas.
```
Listar todos los libros y sus autores
```sql
# Introduce aquí las consultas.
```
Obtener el historial de préstamos de un libro específico
```sql
# Introduce aquí las consultas.
```
Contar cuántos libros han sido prestados en total
```sql
# Introduce aquí las consultas.
```
Listar todos los libros junto con su última edición y estado de disponibilidad
```sql
# Introduce aquí las consultas.

CREATE DATABASE biblioteca;
SHOW DATABASES;
USE biblioteca;
SHOW TABLES;

CREATE TABLE editor (
editor_id INT PRIMARY KEY AUTO_INCREMENT,
name VARCHAR(50) UNIQUE
);

CREATE TABLE genre (
genre_id INT PRIMARY KEY AUTO_INCREMENT,
name VARCHAR(20) UNIQUE
);

CREATE TABLE book (
book_id INT PRIMARY KEY AUTO_INCREMENT,
tittle VARCHAR(100) UNIQUE,
ISBN VARCHAR(30) UNIQUE,
status VARCHAR(20),
edition INT,
publication DATE,
editor_id INT,
genre_id INT,
FOREIGN KEY (editor_id) REFERENCES editor(editor_id),
FOREIGN KEY (genre_id) REFERENCES genre(genre_id)
);

CREATE TABLE book_genre (
bookgenre_id INT PRIMARY KEY AUTO_INCREMENT,
book_id INT,
genre_id INT,
FOREIGN KEY (book_id) REFERENCES book(book_id),
FOREIGN KEY (genre_id) REFERENCES genre(genre_id)
);

CREATE TABLE author (
author_id INT PRIMARY KEY AUTO_INCREMENT,
name VARCHAR(50),
lastname VARCHAR(50),
country VARCHAR(50)
);

CREATE TABLE book_author (
bookauthor_id INT PRIMARY KEY AUTO_INCREMENT,
book_id INT,
author_id INT,
FOREIGN KEY (book_id) REFERENCES book(book_id),
FOREIGN KEY (author_id) REFERENCES author(author_id)
);

CREATE TABLE lend (
lend_id INT PRIMARY KEY AUTO_INCREMENT,
status VARCHAR(30)
);

CREATE TABLE user (
user_id INT PRIMARY KEY AUTO_INCREMENT,
name VARCHAR(50),
lastname VARCHAR(50),
direction VARCHAR(50),
city VARCHAR(30),
email VARCHAR(50) UNIQUE,
phone INT UNIQUE,
date_register DATE
);

CREATE TABLE detail_lend (
detail_id INT PRIMARY KEY AUTO_INCREMENT,
loan DATE,
back DATE,
user_id INT,
lend_id INT,
FOREIGN KEY (user_id) REFERENCES user(user_id),
FOREIGN KEY (lend_id) REFERENCES lend(lend_id)
);

SHOW TABLES;