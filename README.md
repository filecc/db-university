## Eseguire le seguenti query

### Esercizio 12/05/2023

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
```sql
SELECT *
FROM `students`
JOIN `degrees` 
ON `degrees`.`id` = `students`.`degree_id`
WHERE `degrees`.`name` = "corso di laurea in economia";
```
OPPURE 

```sql
SELECT *
FROM `students`
JOIN `degrees` 
ON `degrees`.`id` = `students`.`degree_id`
WHERE `degrees`.`id` = 53;
```
2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
```sql
SELECT *
FROM `degrees`
JOIN `departments` 
ON `departments`.`id` = `degrees`.`department_id`
WHERE `departments`.`name` LIKE '%neuroscienze'
AND `degrees`.`level` = 'magistrale';
```

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
```sql
SELECT *
FROM `courses`
JOIN `course_teacher`
ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `teachers`
ON `teachers`.id = `course_teacher`.`teacher_id`
WHERE `teachers`.`id` = 44;
```
4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il
relativo dipartimento, in ordine alfabetico per cognome e nome
```sql
SELECT `students`.`name`, `students`.`surname`, `students`.`fiscal_code`, `degrees`.`name`, `departments`.`name`
FROM `students`
JOIN `degrees`
ON `students`.`degree_id` = `degrees`.`id`
JOIN `departments`
ON `degrees`.`department_id` = `departments`.id;
```
5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
```sql
SELECT `degrees`.*, `courses`.`name` AS 'course_name',  `courses`.`year`, `teachers`.`name` AS 'teacher_name', `teachers`.`surname` AS 'surname'
FROM `degrees`
JOIN `courses` 
ON `degrees`.id = `courses`.`degree_id`
JOIN `course_teacher`
ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `teachers`
ON `teachers`.`id` = `course_teacher`.`teacher_id`;
```
6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
```sql
SELECT `teachers`.*, `departments`.name
FROM `teachers`
JOIN `course_teacher`
ON `course_teacher`.`teacher_id` = `teachers`.`id`
JOIN `courses`
ON `courses`.id = `course_teacher`.`course_id`
JOIN `degrees`
ON `degrees`.`id` = `courses`.`degree_id`
JOIN `departments`
ON `departments`.`id` = `degrees`.`department_id`
WHERE `departments`.`name` LIKE '%matematica';
````

7. BONUS: Selezionare per ogni studente quanti tentativi d’esame ha sostenuto per
    superare ciascuno dei suoi esami
```sql
SELECT `exam_student`.`exam_id`, `students`.`fiscal_code`, `students`.`name`, `students`.`surname`, COUNT(`exam_student`.`exam_id`) AS `exams_count`
FROM `students`
JOIN `exam_student` 
ON `students`.`id` = `exam_student`.`student_id`
JOIN `exams`
ON `exam_student`.`exam_id` = `exams`.`id`
GROUP BY `students`.`id`, `exam_student`.`exam_id`;
```
8. Contare quanti iscritti ci sono stati ogni anno
```sql
SELECT YEAR(`students`.`enrolment_date`) AS 'year',  COUNT(YEAR(`students`.`enrolment_date`)) AS 'total_enrollments'
FROM `students`
GROUP BY YEAR(`students`.`enrolment_date`);
```
9. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
```sql
SELECT `teachers`.`office_address`, COUNT(`teachers`.`office_address`)
FROM `teachers`
GROUP BY `teachers`.`office_address`;
```
10. Calcolare la media dei voti di ogni appello d'esame
```sql
SELECT `exam_student`.`exam_id`, AVG(`exam_student`.`vote`)
FROM `exam_student`
GROUP BY `exam_student`.`exam_id`
ORDER BY AVG(`exam_student`.`vote`) ASC;
```
11. Contare quanti corsi di laurea ci sono per ogni dipartimento
```sql
SELECT  `departments`.`name`, COUNT(`degrees`.`id`) AS 'CDL'
FROM `degrees`
JOIN `departments`
ON `departments`.`id` = `degrees`.`department_id`
GROUP BY `departments`.`name`
ORDER BY COUNT(`degrees`.`id`) DESC;
```


### Esercizio 11/05/2023


1. Selezionare tutti gli studenti nati nel 1990 (160)

```sql
SELECT * 
FROM `students` 
WHERE YEAR(`date_of_birth`) = 1990;
```

2. Selezionare tutti i corsi che valgono più di 10 crediti (479)

```sql
SELECT * 
FROM `courses` 
WHERE `cfu` > 10;
```

3. Selezionare tutti gli studenti che hanno più di 30 anni

```sql
SELECT * 
FROM `students` 
WHERE  YEAR(CURRENT_DATE) - YEAR(`date_of_birth`) >= 30;
```

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
laurea (286)

```sql
SELECT * 
FROM `courses` 
WHERE `period` = 'I semestre'
AND `year` = 1;
```

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
20/06/2020 (21)

```sql
SELECT * 
FROM `exams` 
WHERE `date` = '2020/06/20'
AND `hour` > '14:00:00';
```

6. Selezionare tutti i corsi di laurea magistrale (38)
```sql
SELECT * 
FROM `degrees` 
WHERE `level` = 'magistrale';
```

7. Da quanti dipartimenti è composta l'università? (12)
```sql
SELECT COUNT(`id`) AS `numbers_of_departments` FROM `departments`;
```
8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
```sql
SELECT COUNT(`id`) AS `no_phone` 
FROM `teachers`
WHERE `phone` IS NULL;
```


