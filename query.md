<!-- 1. Selezionare tutti gli studenti nati nel 1990 (160)
2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
3. Selezionare tutti gli studenti che hanno più di 30 anni
4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
laurea (286)
5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
20/06/2020 (21)
6. Selezionare tutti i corsi di laurea magistrale (38)
7. Da quanti dipartimenti è composta l'università? (12)
8. Quanti sono gli insegnanti che non hanno un numero di telefono?  -->


1-

SELECT * FROM students WHERE date_of_birth LIKE '1990-%';

2-

SELECT * FROM `courses` WHERE `cfu` > 10;

3-

SELECT * FROM students WHERE TIMESTAMPDIFF(YEAR, date_of_birth, CURRENT_DATE) >= 30;

4-

SELECT * FROM `courses` WHERE `period` = 'I semestre' AND `year` = 1;

5-

SELECT * FROM `exams` WHERE `date` = '2020-06-20' AND `hour` >= '14:00:00 ';

6-

SELECT * FROM `degrees` WHERE `name` LIKE '%Corso di Laurea Magistrale%';

7-

SELECT * FROM `departments` WHERE `id` IS NOT NULL;

8-

SELECT * FROM `teachers` WHERE `phone` IS NULL;


<!-- Group by:
Contare quanti iscritti ci sono stati ogni anno

Contare gli insegnanti che hanno l'ufficio nello stesso edificio


Calcolare la media dei voti di ogni appello d'esame


Contare quanti corsi di laurea ci sono per ogni dipartimento


Joins:
Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze


Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)


Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome


Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti


Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18. -->

- Contare quanti iscritti ci sono stati ogni anno

SELECT COUNT(*), YEAR(enrolment_date) FROM students GROUP BY YEAR(enrolment_date);

- Contare gli insegnanti che hanno l'ufficio nello stesso edificio

SELECT office_address, COUNT(office_address) FROM teachers GROUP BY office_address;

- Calcolare la media dei voti di ogni appello d'esame

SELECT exam_id, AVG(vote) FROM exam_student GROUP BY exam_id;

- Contare quanti corsi di laurea ci sono per ogni dipartimento

SELECT department_id, COUNT(name) FROM degrees GROUP BY department_id;

joins:

- Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

SELECT students.name, students.surname, degrees.name AS degree_name
FROM students
JOIN degrees on students.degree_id = degrees.id
WHERE degrees.name = 'Corso di Laurea in Economia';

- Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

SELECT degrees.level, degrees.name AS degree_name, departments.name AS department_name FROM degrees
JOIN departments ON departments.id = 7
WHERE degrees.level = 'magistrale';


- Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)


SELECT teacher_id, teachers.name AS teacher_name, course_teacher.course_id AS course_id FROM course_teacher
JOIN teachers ON teacher_id = 44
WHERE teachers.id = 44;

- Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

SELECT students.name, students.surname, degrees.name AS degree_name, degrees.level AS degree_level, departments.name AS department_name
FROM students
JOIN degrees ON degree_id = degrees.id
JOIN departments ON department_id = departments.id
ORDER BY students.name, students.surname ASC;

- Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

SELECT degrees.name AS degree_name, courses.name AS course_name, teachers.name AS teacher_name, teachers.surname AS teacher_surname
FROM course_teacher
JOIN courses ON course_id = courses.id
JOIN degrees ON degree_id = degrees.id
JOIN teachers ON teacher_id = teachers.id

- Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

## errore

SELECT degrees.name AS degree_name, courses.name AS course_name, teachers.name AS teacher_name, teachers.surname AS teacher_surname
FROM course_teacher
JOIN courses ON course_id = courses.id
JOIN degrees ON degree_id = degrees.id
JOIN teachers ON teacher_id = teachers.id;