# bibliotecaSQL

# Modelo conceptual
![alt text](<Modelo Conceptual.png>)

# Modelo Fisico
Al realizar la identificación de variables se detecta la necesidad de normalizar el genero del libro.

![alt text](<Modelo  Físico.png>)

# Consultas

1. Listar todos los libros disponibles
```sql
SELECT tittle FROM book;
```
2. Buscar libros por género
```sql
SELECT b.tittle, g.name FROM book AS b JOIN book_genre AS bg ON b.book_id = bg.book_id JOIN genre AS g ON bg.genre_id = g.genre_id ORDER BY g.name, b.tittle;
```
3. Obtener información de un libro por ISBN
```sql
SELECT tittle, edition, publication FROM book WHERE ISBN = '9788408014225'; -- Reemplaza el codigo ISBN que deseas consultar
```
4. Contar el número de libros en la biblioteca
```sql
SELECT COUNT(tittle) AS total_libros FROM book;
```
5. Listar todos los autores
```sql
SELECT name,lastname FROM author;
```
6. Buscar autores por nombre
```sql
SELECT name, lastname FROM author WHERE name='carlos'; -- Reemplaza el nombre de autor que deseas consultar
```
7. Obtener todos los libros de un autor específico
```sql
SELECT b.tittle FROM book AS b JOIN book_author AS ba ON b.book_id = ba.book_id JOIN author AS a ON ba.author_id = a.author_id WHERE a.name = 'carlos'; -- Reemplaza el nombre de autor que deseas consultar
```
8. Listar todas las ediciones de un libro
```sql
SELECT tittle, edition FROM book WHERE tittle = 'La sombra del viento'; -- Reemplaza el nombre del libro que deseas consultar
```
9. Obtener la última edición de un libro
```sql
SELECT tittle, MAX(edition) AS ultima_edition FROM book WHERE tittle = 'La sombra del viento' GROUP BY tittle; -- Reemplaza el nombre del libro que deseas consultar
```
10. Contar cuántas ediciones hay de un libro específico
```sql
SELECT COUNT(edition) AS total_ediciones FROM book WHERE tittle = 'La sombra del viento'; -- Reemplaza el nombre del libro que deseas consultar
```
11. Listar todas las transacciones de préstamo
```sql
SELECT lend_id,status FROM lend;
```
12. Obtener los libros prestados actualmente
```sql
SELECT b.tittle, l.status FROM book AS b JOIN detail_lend AS dl ON b.book_id = dl.book_id JOIN lend AS l ON dl.lend_id = l.lend_id WHERE l.status = 'Prestado'; 
```
13. Contar el número de transacciones de un miembro específico
```sql
SELECT COUNT(dl.detail_id) AS total_prestamos FROM detail_lend AS dl JOIN user AS u ON dl.user_id = u.user_id WHERE u.name = 'Ana';  -- Reemplaza el nombre del usuario que deseas consultar
```
14. Listar todos los miembros de la biblioteca
```sql
SELECT name,lastname FROM user;
```
15. Buscar un miembro por nombre:
```sql
SELECT name,lastname FROM user WHERE name LIKE '%Ana%'; -- Reemplaza el nombre del usuario que deseas consultar
```
16. Obtener las transacciones de un miembro específico
```sql
SELECT dl.*, b.tittle FROM detail_lend AS dl JOIN book AS b ON dl.book_id = b.book_id JOIN user AS u ON dl.user_id = u.user_id WHERE u.name = 'Ana'; -- Reemplaza el nombre del usuario que deseas consultar 
```
17. Listar todos los libros y sus autores
```sql
SELECT b.tittle, a.name, a.lastname FROM book AS b JOIN book_author AS ba ON b.book_id = ba.book_id JOIN author AS a ON ba.author_id = a.author_id;
```
18. Obtener el historial de préstamos de un libro específico
```sql
SELECT dl.loan, dl.back, u.name, u.lastname FROM detail_lend AS dl JOIN book AS b ON dl.book_id = b.book_id JOIN user AS u ON dl.user_id = u.user_id WHERE b.tittle = 'La sombre del viento'; -- Reemplaza el nombre del libro que deseas consultar 
```
19. Contar cuántos libros han sido prestados en total
```sql
SELECT COUNT(dl.detail_id) AS total_libros_prestados FROM detail_lend AS dl JOIN lend AS l ON dl.lend_id = l.lend_id WHERE l.status = 'Prestado';
```
20. Listar todos los libros junto con su última edición y estado de disponibilidad
```sql
SELECT tittle,MAX(edition) AS ultima_edicion,status FROM book GROUP BY tittle; 
```

# Creación de Base de datos
```sql
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
status VARCHAR(30),
book_id INT,
FOREIGN KEY (book_id) REFERENCES book(book_id)
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
book_id INT,
FOREIGN KEY (user_id) REFERENCES user(user_id),
FOREIGN KEY (lend_id) REFERENCES lend(lend_id),
FOREIGN KEY (book_id) REFERENCES book(book_id)
);

SHOW TABLES;
```

# Datos ejemplo de Base de datos
```sql
INSERT INTO editor (name) VALUES
('Editorial Planeta'),
('Editorial Penguin Random House'),
('Editorial Alianza'),
('Editorial Santillana'),
('Editorial Anagrama');

INSERT INTO genre (name) VALUES
('Ficción'),
('No Ficción'),
('Misterio'),
('Ciencia Ficción'),
('Fantástico');

INSERT INTO book (tittle, ISBN, status, edition, publication, editor_id, genre_id) VALUES
('La sombra del viento', '9788408014225', 'Disponible', 1, '2001-03-22', 1, 1),
('1984', '9780451524935', 'Disponible', 1, '1949-06-08', 2, 2),
('Cien años de soledad', '9780307474728', 'Disponible', 1, '1967-06-05', 3, 1),
('El nombre de la rosa', '9788435065167', 'Disponible', 1, '1980-01-01', 4, 3),
('El retrato de Dorian Gray', '9780141441744', 'Disponible', 1, '1890-06-01', 5, 4);

INSERT INTO book_genre (book_id, genre_id) VALUES
(1, 1),
(2, 2),
(3, 1),
(4, 3),
(5, 4);

INSERT INTO author (name, lastname, country) VALUES
('Carlos', 'Ruiz Zafón', 'España'),
('George', 'Orwell', 'Reino Unido'),
('Gabriel García', 'Márquez', 'Colombia'),
('Umberto', 'Eco', 'Italia'),
('Oscar', 'Wilde', 'Irlanda');

INSERT INTO book_author (book_id, author_id) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5);

INSERT INTO lend (status, book_id) VALUES
('Devuelto', 1),
('Prestado', 2),
('Devuelto', 3),
('Prestado', 4),
('Prestado', 5);

INSERT INTO user (name, lastname, direction, city, email, phone, date_register) VALUES
('Ana', 'García', 'Calle Ficticia 123', 'Madrid', 'ana@gmail.com', 123456789, '2025-03-01'),
('Luis', 'Martínez', 'Avenida Siempre Viva 456', 'Barcelona', 'luis@mail.com', 987654321, '2025-03-05'),
('Carlos', 'Hernández', 'Calle del Sol 789', 'Sevilla', 'carlos_hernandez@gmail.com', 456789123, '2025-03-06'),
('Sara', 'López', 'Calle Luna 101', 'Valencia', 'sara@yahoo.com', 654987321, '2025-03-02'),
('Marta', 'Pérez', 'Calle Estrella 555', 'Madrid', 'marta@hotmail.com', 321654987, '2025-03-04');

INSERT INTO detail_lend (loan, back, user_id, lend_id) VALUES
('2025-03-01', '2025-03-10', 1, 1),
('2025-03-02', '2025-03-11', 2, 2),
('2025-03-03', '2025-03-12', 3, 3),
('2025-03-04', '2025-03-13', 4, 4),
('2025-03-05', '2025-03-14', 5, 5);
```