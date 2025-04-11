Modellizzare la struttura di un database per memorizzare tutti i dati riguardanti una università:
sono presenti diversi _Dipartimenti_ (es.: Lettere e Filosofia, Matematica, Ingegneria ecc.);
ogni _Dipartimento_ offre più _Corsi di Laurea_ (es.: Civiltà e Letterature Classiche, Informatica, Ingegneria Elettronica ecc..)
ogni _Corso di Laurea_ prevede diversi _Corsi_ (es.: Letteratura Latina, Sistemi Operativi 1, Analisi Matematica 2 ecc.);
ogni _Corso_ può essere tenuto da diversi _Insegnanti_;
ogni _Corso_ prevede più appelli d'_Esame_;
ogni _Studente_ è iscritto ad un solo _Corso di Laurea_;
ogni _Studente_ può iscriversi a più appelli di _Esame_;
per ogni appello d'_Esame_ a cui lo _Studente_ ha partecipato, è necessario memorizzare il voto ottenuto, anche se non sufficiente. Pensiamo a quali entità (tabelle) creare per il nostro database e cerchiamo poi di stabilirne le relazioni. Infine, andiamo a definire le colonne e i tipi di dato di ogni tabella.

Dopo aver creato un nuovo database nel vostro MySQL Workbench e aver importato lo schema allegato, eseguite le query di seguito:
:puntina: Cosa devi fare:

1. Selezionare tutti gli studenti nati nel 1990 (160)
2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
3. Selezionare tutti gli studenti che hanno più di 30 anni
4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)
5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)
6. Selezionare tutti i corsi di laurea magistrale (38)
7. Da quanti dipartimenti è composta l'università? (12)
8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
   :puntare_a_destra: Cosa devi consegnare?
   Dopo aver testato le vostre query con MySQL Workbench, riportatele in un file txt o md e caricatelo nella vostra repo.

## table name `department`

## columns

id: (BIGINT) primary_key auto_increment NOT NULL
department_id: (BIGINT) foreign_key NOT NULL
teacher_id: (BIGINT) foreign_key NOT NULL
course_id: (BIGINT) foreign_key NOT NULL
exam_id: (BIGINT) foreign_key NOT NULL
name: VARCHAR(255) NOT NULL

## table name `course`

## columns

id: (BIGINT) primary_key auto_increment NOT NULL
course_id: (BIGINT) foreign_key NOT NULL
name: VARCHAR(255) NOT NULL

## table name `subject`

## columns

id: (BIGINT) primary_key auto_increment NOT NULL
subject_id: (BIGINT) foreign_key NOT NULL
teacher_id: (BIGINT) foreign_key NOT NULL
course_id: (BIGINT) foreign_key NOT NULL
department_id: (BIGINT) foreign_key NOT NULL
name: VARCHAR(255) NOT NULL

## table name `student`

## columns

id: (BIGINT) primary_key auto_increment NOT NULL
subject_id: (BIGINT) foreign_key NOT NULL
course_id: (BIGINT) foreign_key NOT NULL
department_id: (BIGINT) foreign_key NOT NULL
name: VARCHAR(255) NOT NULL
surname: VARCHAR(255) NOT NULL
email: VARCHAR(255) UNIQUE NOT NULL
date_of_birth: DATE() NOT NULL
registration_number: SMALLINT UNIQUE NOT NULL
year: YEAR() NOT NULL

## table name `teacher`

## columns

id (BIGINT) primary_key auto_increment NOT NULL
subject_id: (BIGINT) foreign_key NOT NULL
course_id: (BIGINT) foreign_key NOT NULL
department_id: (BIGINT) foreign_key NOT NULL
name: VARCHAR(255) NOT NULL
surname: VARCHAR(255) NOT NULL
email: VARCHAR(255) UNIQUE NOT NULL
date_of_birth: DATE() NULL
role: VARCHAR(50)

## table name `exam`

## columns

id (BIGINT) primary_key auto_increment NOT NULL
subject_id: (BIGINT) foreign_key NOT NULL
teacher_id: foreign_key NOT NULL
date: DATE() NOT NULL
numer_of_student: SMALLINT NOT NULL

## table name `vote`

## columns

id (BIGINT) primary_key auto_increment NOT NULL
exam_id:(BIGINT) foreign_key NOT NULL
student_id:(BIGINT) foreign_key NOT NULL
teacher_id:(BIGINT) foreign_key NOT NULL
vote: TINYINT(2) NOT NULL
honor: TINYINT DEFAULT(0)

---

SELECT \*
FROM `students` WHERE YEAR(date\*of_birth) = 1990;

SELECT \* FROM `courses` WHERE CFU > 10

SELECT \_ FROM `students` WHERE YEAR(date\*of_birth) <= 1995;

SELECT \* FROM `courses`
WHERE `year` = 1
AND `period` = 'I semestre'

SELECT \* FROM `exams`
WHERE `date` = '2020-06-20'
AND `hour` >= '14:00:00'

SELECT \_ FROM `degrees`
WHERE `level` = 'magistrale'

SELECT COUNT('id') FROM `departments`

SELECT COUNT('phone') from `teachers`
WHERE `phone` IS NULL

SELECT YEAR(`enrolment_date`) AS anno,
COUNT(\*) AS numero_iscritti
FROM `students`
GROUP BY YEAR(`enrolment_date`)

SELECT `office_address` AS ufficio,
COUNT(\*) AS numero_prof
FROM `teachers`
GROUP BY (`office_address`)

SELECT AVG(`exam_student`.`vote`) AS media_voti, `courses`.`name` AS `course_name`, `degrees`.`name` AS `degree_name`
FROM `exam_student`
JOIN `exams` ON `exam_student`.`exam_id` = `exams`.`id`
JOIN `courses` ON `exams`.`course_id` = `courses`.`id`
JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id`
GROUP BY `courses`.`id`

departments.name AS nome_dipartimento,
COUNT(degrees.id) AS numero_corsi FROM `departments`
JOIN `degrees` ON `degrees`.`department_id` = `departments`.`id`
GROUP BY `departments`.`id`
