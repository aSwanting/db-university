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
4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
   laurea (286)
   ```MYSQL
   SELECT * FROM `courses`
   WHERE `period` = 'I semestre' AND `year` = '1';
   ```
5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
   20/06/2020 (21)
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
## Part 2
### Group
1. Contare quanti iscritti ci sono stati ogni anno
   ```MYSQL
   SELECT COUNT(id), `enrolment_date`
   FROM `students`
   GROUP BY `enrolment_date`
   ORDER BY `enrolment_date` ASC;
   ```
2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
3. Calcolare la media dei voti di ogni appello d'esame
4. Contare quanti corsi di laurea ci sono per ogni dipartimento
