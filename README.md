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

1. Contare quanti iscritti ci sono stati ogni anno
   ```MYSQL
   SELECT `enrolment_date`, COUNT(`id`)
   FROM `students`
   GROUP BY `enrolment_date`
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

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
   ```MYSQL
   SELECT *
   FROM `students`
   INNER JOIN `degrees`
   ON `students`.`degree_id` = `degrees`.`id`
   WHERE `degrees`.`name` = "Corso di Laurea in Economia";
   ```
2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

   ```MYSQL

   ```

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

   ```MYSQL

   ```

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

   ```MYSQL

   ```

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

   ```MYSQL

   ```

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

   ```MYSQL

   ```

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente,filtrare i tentativi con voto minimo 18.

   ```MYSQL

   ```
