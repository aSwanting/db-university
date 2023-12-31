# db-university

## Part 1

1. Selezionare tutti gli studenti nati nel 1990 (160)
   ```MYSQL
   SELECT * FROM `students`
   WHERE `date_of_birth` LIKE '1990%';
   ```
2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
   ```MYSQL
   SELECT * FROM `courses`
   WHERE `cfu` > '10';
   ```
3. Selezionare tutti gli studenti che hanno più di 30 anni
   ```MYSQL
   SELECT * FROM `students`
   WHERE `date_of_birth` < DATE_SUB(CURDATE(), INTERVAL 30 YEAR);
   ```
4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)
   ```MYSQL
   SELECT * FROM `courses`
   WHERE `period` = 'I semestre' AND `year` = '1';
   ```
5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)
   ```MYSQL
   SELECT * FROM `exams`
   WHERE `date` = '2020-06-20' AND `hour` > '14:00:00';
   ```
6. Selezionare tutti i corsi di laurea magistrale (38)
   ```MYSQL
   SELECT * FROM `degrees`
   WHERE `level` = 'magistrale';
   ```
7. Da quanti dipartimenti è composta l'università? (12)
   ```MYSQL
   SELECT COUNT(DISTINCT `name`)
   FROM `departments`;
   ```
8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
   ```MYSQL
   SELECT COUNT(*) FROM `teachers`
   WHERE `phone` IS NULL;
   ```

## Part 2.1

### Group

1. Contare quanti iscritti ci sono stati ogni anno (correzione: mancava YEAR)
   ```MYSQL
   SELECT YEAR(`enrolment_date`), COUNT(`id`)
   FROM `students`
   GROUP BY YEAR(`enrolment_date`);
   ```
2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
   ```MYSQL
   SELECT `office_address`, COUNT(`id`)
   FROM `teachers`
   GROUP BY `office_address`
   ```
3. Calcolare la media dei voti di ogni appello d'esame
   ```MYSQL
   SELECT `exam_id`, AVG(`vote`)
   FROM `exam_student`
   GROUP BY `exam_id`;
   ```
4. Contare quanti corsi di laurea ci sono per ogni dipartimento
   ```MYSQL
   SELECT `department_id`, COUNT(`id`)
   FROM `degrees`
   GROUP BY `department_id`;
   ```

## Part 2.2

### Join

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia (correction: add degree_name)
   ```MYSQL
   SELECT `students`.*, `degrees`.`name` AS `degree_name`
   FROM `students`
   INNER JOIN `degrees`
   ON `students`.`degree_id` = `degrees`.`id`
   WHERE `degrees`.`name` = "Corso di Laurea in Economia";
   ```
2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze (correction: add department_name)
   ```MYSQL
   SELECT `degrees`.*, `departments`.`name` AS `department_name`
   FROM `degrees`
   INNER JOIN `departments`
   ON `degrees`.`department_id` = `departments`.`id`
   WHERE `degrees`.`level` = "magistrale"
   AND `departments`.`name` LIKE '%neuroscienze';
   ```
3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44) (correction: column cleanup)
   ```MYSQL
   SELECT `courses`.*, CONCAT(`teachers`.`name`, ' ', `teachers`.`surname`) AS `teacher`, `teachers`.`id` AS `teacher_id`
   FROM `teachers`
   INNER JOIN `course_teacher`
   ON `teachers`.`id` = `course_teacher`.`teacher_id`
   INNER JOIN `courses`
   ON `course_teacher`.`course_id` = `courses`.`id`
   WHERE `teachers`.`name` = 'fulvio' AND `teachers`.`surname` = 'amato';
   ```
4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome (correction: column cleanup)
   ```MYSQL
   SELECT `students`.`name`,`students`.`surname`,  `degrees`.`name` AS `degree_name`, `degrees`.`level`, `degrees`.`address`, `degrees`.`email`, `degrees`.`website`, `departments`.`name` AS `department_name`
   FROM `students`
   INNER JOIN `degrees`
   ON `students`.`id` = `degrees`.`id`
   INNER JOIN `departments`
   ON `degrees`.`department_id` = `departments`.`id`
   ORDER BY `students`.`surname` ASC, `students`.`name` ASC;
   ```
5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti (correction: column cleanup)
   ```MYSQL
   SELECT `degrees`.`name` AS `degree_name`, `courses`.`name` AS `course_name`, CONCAT(`teachers`.`name`, ' ', `teachers`.`surname`) AS `teacher`
   FROM `degrees`
   INNER JOIN `courses`
   ON `degrees`.`id` = `courses`.`degree_id`
   INNER JOIN `course_teacher`
   ON `courses`.`id` = `course_teacher`.`course_id`
   INNER JOIN `teachers`
   ON `course_teacher`.`teacher_id` = `teachers`.`id`
   ORDER BY `degrees`.`name` ASC;
   ```
6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
   ```MYSQL
   SELECT DISTINCT CONCAT(`teachers`.`name`,' ', `teachers`.`surname`) AS `full_name`, `teachers`.`email`
   FROM `teachers`
   INNER JOIN `course_teacher`
   ON `teachers`.`id` = `course_teacher`.`teacher_id`
   INNER JOIN `courses`
   ON `course_teacher`.`course_id` = `courses`.`id`
   INNER JOIN `degrees`
   ON `courses`.`degree_id` = `degrees`.`id`
   INNER JOIN `departments`
   ON `degrees`.`department_id` = `departments`.`id`
   WHERE `departments`.`name` = 'Dipartimento di Matematica'
   ORDER BY `full_name` ASC;
   ```
7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i TENTATIVI con voto minimo 18.
   ```MYSQL
   SELECT `students`.`name`, `students`.`surname`, `student_id`, `courses`.`name` AS `course_name`, COUNT(`exams`.`course_id`) AS `attempts`, MAX(`exam_student`.`vote`) AS highest_vote
   FROM `students`
   INNER JOIN `exam_student`
   ON `students`.`id` = `exam_student`.`student_id`
   INNER JOIN `exams`
   ON `exam_student`.`exam_id` = `exams`.`id`
   INNER JOIN `courses`
   ON `exams`.`course_id` = `courses`.`id`
   WHERE `vote` >= 18
   GROUP BY `students`.`name`, `students`.`surname`, `course_name`  , `student_id`
   ORDER BY `exam_student`.`student_id` ASC;
   ```
8. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i RISULTATI con voto minimo 18.
   ```MYSQL
   SELECT `students`.`name`, `students`.`surname`, `student_id`, `courses`.`name` AS `course_name`, COUNT(`exams`.`course_id`) AS `attempts`, MAX(`exam_student`.`vote`) AS highest_vote
   FROM `students`
   INNER JOIN `exam_student`
   ON `students`.`id` = `exam_student`.`student_id`
   INNER JOIN `exams`
   ON `exam_student`.`exam_id` = `exams`.`id`
   INNER JOIN `courses`
   ON `exams`.`course_id` = `courses`.`id`
   GROUP BY `students`.`name`, `students`.`surname`, `course_name`  , `student_id`
   HAVING(`highest_vote`) >= 18
   ORDER BY `exam_student`.`student_id` ASC;
   ```
