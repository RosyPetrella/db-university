Modellizzare la struttura di un database per memorizzare tutti i dati riguardanti una università:
sono presenti diversi _Dipartimenti_ (es.: Lettere e Filosofia, Matematica, Ingegneria ecc.);
ogni _Dipartimento_ offre più _Corsi di Laurea_ (es.: Civiltà e Letterature Classiche, Informatica, Ingegneria Elettronica ecc..)
ogni _Corso di Laurea_ prevede diversi _Corsi_ (es.: Letteratura Latina, Sistemi Operativi 1, Analisi Matematica 2 ecc.);
ogni _Corso_ può essere tenuto da diversi _Insegnanti_;
ogni _Corso_ prevede più appelli d'_Esame_;
ogni _Studente_ è iscritto ad un solo _Corso di Laurea_;
ogni _Studente_ può iscriversi a più appelli di _Esame_;
per ogni appello d'_Esame_ a cui lo _Studente_ ha partecipato, è necessario memorizzare il voto ottenuto, anche se non sufficiente. Pensiamo a quali entità (tabelle) creare per il nostro database e cerchiamo poi di stabilirne le relazioni. Infine, andiamo a definire le colonne e i tipi di dato di ogni tabella.

## table name `department`

## columns

id: (BIGINT) primary_key auto_increment NOT NULL
department_id:
name:

## table name `course`

## columns

id: (BIGINT) primary_key auto_increment NOT NULL
course_id:
name:

## table name `subject`

## columns

id: (BIGINT) primary_key auto_increment NOT NULL
subject_id:
teacher_id:
course_id:
department_id:
name:

## table name `student`

## columns

id: (BIGINT) primary_key auto_increment NOT NULL
subject_id:
course_id:
department_id:
name:
surname:
email:
date of birth:
registration_number:
year: YEAR()

## table name `teacher`

## columns

id (BIGINT) primary_key auto_increment NOT NULL
subject_id
course_id
department_id
name
surname
email
date of birth
role

## table name `exam`

## columns

id (BIGINT) primary_key auto_increment NOT NULL
subject_id
date

## table name `vote`

## columns

id (BIGINT) primary_key auto_increment NOT NULL
subject_id:
vote: TINYINT(2)
honor: TINYINT DEFAULT(0)
