# db-university

1. Selezionare tutti gli studenti nati nel 1990 (160)
   ```MYSQL
   SELECT * FROM `students`
   WHERE date_of_birth LIKE '1990%';
   ```
2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
   ```MYSQL
   SELECT * FROM `courses`
   WHERE cfu > '10';
   ```
