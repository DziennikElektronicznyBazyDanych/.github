# Progress - school ediary 
Some humble guy once said:
```
Science is the power key
```
That is why we want to make the learning experience more attractive for students and help teachers organize their work.
[in progress]

## Authors
* Ewelina Flisak
* Piotr Socała
* Arkadiusz Tyrański

## Dev stack
* Front: React
* Database: Postgresql
* Backend: NodeJs + Express.js
* Endpoint testing: Postman

## Repositories
* [Frontend](https://github.com/DziennikElektronicznyBazyDanych/frontend)
* [Backend](https://github.com/DziennikElektronicznyBazyDanych/backend)
* [Database](https://github.com/DziennikElektronicznyBazyDanych/database)

## Run application
## Client application views
### Login screen
![image](https://user-images.githubusercontent.com/72259657/173589869-a5e03860-360a-447d-ae12-398a35d8c1e8.png)
### Register screen
![image](https://user-images.githubusercontent.com/72259657/173589934-2d071ccf-725e-4a6d-8c93-5ee95ea362ae.png)
### Student home page
![image](https://user-images.githubusercontent.com/72259657/173590294-23b117bc-01c8-46bf-a965-e1db14328197.png)
### Student profile
![image](https://user-images.githubusercontent.com/72259657/173590369-57bcb860-1232-4fc5-8f8a-c0819fa4c32c.png)
### Student schedule
![image](https://user-images.githubusercontent.com/72259657/173590506-1e126048-3e30-44f5-bf67-acab1d85b0b4.png)
### Student grades
![image](https://user-images.githubusercontent.com/72259657/173590657-0b21a5cb-8187-42d3-bdb2-eddb3c3bdb1e.png)
### Student tests
![image](https://user-images.githubusercontent.com/72259657/173590773-2cb35dfc-56e2-4488-a6f6-1c5da78a3694.png)
### Student attendance summary
![image](https://user-images.githubusercontent.com/72259657/173590898-405f2846-9ff0-4886-b1c2-8e51013eb1d4.png)
### Teacher profile
![image](https://user-images.githubusercontent.com/72259657/173591682-c1474d3e-114b-49d6-a8a5-bf2e34b658a8.png)
### Teacher home page
![image](https://user-images.githubusercontent.com/72259657/173591303-b3ceb2c6-762a-497b-a746-0db3915eec66.png)
### Teacher schedule
![image](https://user-images.githubusercontent.com/72259657/173591420-cde1e240-cbd5-4776-baa0-6ee3fb0cf26c.png)
### Teacher lesson panel
![image](https://user-images.githubusercontent.com/72259657/173591514-3af89cc7-4a61-4b5c-a1fc-492970040ff4.png)
### Teacher add grade modal
![image](https://user-images.githubusercontent.com/72259657/173592000-f572f913-285b-4e1b-a203-c81c75699df9.png)

## Database views, procedures and functions
## API Endpoints

# Autentykacja
## ✔️ auth/login
- metoda: POST
- body:
  - email::varchar(50),
  - password::varchar(50)
- output: 
```
{
	person_id: number,
	first_name: string,
	last_name: string,
	sex: string,
	birth_date: date,
	city: string,
	postal_code: string,
	email: string,
	start_date: string, 	# zmiana
	end_date: string,	# zmiana
	class_id: number,
	class_name: string,	# zmiana
	account_type: string,	# zmiana
}
```
- przykładowe wykorzystanie funkcji z bazy danych:
```
SELECT * FROM func_login('karolina_jakubowska@gmail.com', 'KarolinaJakubowska834'); 	# zmiana - logowanie studenta
SELECT * FROM func_login('łucja_grabowska@gmail.com', 'ŁucjaGrabowska159');		# zmiana - logowanie teachera
```

- te dane nie powinny zadziałać
```
SELECT * FROM func_login_student('karolina_jakubowska@gmail.com', 'Alamakota1234');
```
## ✔️ auth/register
- metoda: POST
- body: 
  - first_name::varchar(20),
  - last_name::varchar(50),
  - sex::bit,
  - birth_date::date,
  - city::varchar(20),
  - postal_code::varchar(6), 
  - email::varchar(50), 
  - password::varchar(50), 
  - type: varchar(1),
- output: 
```
{	 
	success: boolean,
	message: string,
}
```
- przykładowe wykorzystanie funkcji z bazy danych:
```
-- dodanie studenta
SELECT * FROM func_register('Zenon', 'Krasicki', 0::bit, '2000-04-15', 'Kraków', '31-852', 'zenkras@gmail.com', 'zenKras1234', 'S');

-- dodanie nauczyciela
SELECT * FROM func_register('Paulina', 'Sowa', 1::bit, '1980-09-02', 'Kraków', '31-551', 'paulsow@gmail.com', 'paulSow1234', 'T');
```

# Student
## ✔️ student/schedule
- metoda: POST
- body:
  - class_id::int,
  - year::varchar(4),
  - semester::bit,
  - date_from::date,
  - date_to::date
- output: 
```
[
	{
		lesson_instance_id: number,
		lesson_id: number,
		leading_teacher_id: number,
		first_name: string,
		last_name: string,
		day_id: number,
		day_name: string,
		classroom_id: number,
		classroom_name: string
		start_time: string,
		end_time: string,
		course_id: number,
		course_name: string,
		is_cancelled: boolean,
		lesson_date: string,
	}
]
```
- przykładowe wykorzystanie funkcji z bazy danych:
```
SELECT * FROM func_class_schedule_between(1,'2022',0::bit, '2022-09-05', '2022-09-09');
```

## ✔️ student/tests
- metoda: POST
- body:
  - class_id::int,
  - year::varchar(4),
- output:
```
[
  {
    test_id: number,
    material_scope: string,
    test_weight: number,
    teacher_id: number,
    first_name: string,
    last_name: string,
    is_cancelled: boolean,
    lesson_date: string
  }
]
```

- przykładowe wykorzystanie z bazy danych:
```
SELECT * FROM func_get_test_for_class(1);
```

## ✔️ grades
- metoda: POST
- body:
  - student_id::int,
  - year::varchar(4)
- output:
```
[
  {
    course_id: number,
    course_name: string,
    grades: [
    	grade_id: number,
      lesson_instance_id: number,
      grade: number,
      grade_type: string,
      weight: number,
      description: string
    ]
  }
]
```

- przykładowe wykorzystanie funkcji z bazy danych:
```
SELECT * FROM func_get_enrolled_courses(20);
SELECT * FROM func_student_grades_per_course(20, '2022');
```
Pierwsza funkcja jest potrzebna żeby pobrać listę przedmiotów, na które dany student jest zapisany, druga po to, żeby pobrać oceny.

## ✔️ student/attendance
- metoda: POST
- body:
  - student_id::int,
  - year::varchar(4)
  - semester::bit
- output:
```
 {
    status: string,
    presence: number, 
    absence: number
 }
```

- przykładowe wykorzystanie funkcji z bazy danych:
```
SELECT * FROM func_student_attendance_all(16::int, '2022'::varchar(4), 0::bit);
```

# Nauczyciel
## ✔️ teacher/schedule
- metoda: POST
- body:
  - teacher_id::int,
  - year::varchar(4),
  - semester::bit,
  - date_from::date,
  - date_to::date
- output: 
```
[
	{
		lesson_instance_id: number,
		lesson_id: number,
		class_id: number,
		day_id: number,
		day_name: string,
		classroom_id: number,
		classroom_name: string
		start_time: string,
		end_time: string,
		course_id: number,
		course_name: string,
		is_cancelled: boolean,
		lesson_date: string,
	}
]
```
- przykładowe wykorzystanie funkcji z bazy danych:
```
SELECT * FROM func_teacher_schedule_between(1, '2022', 0::bit, '2022-09-05', '2022-09-09');
```

## ✔️ student/expell/:student_id
- metoda: GET
- output:
```
 {
    	status: string,
 }
```

- przykładowe wykorzystanie funkcji z bazy danych:
```
SELECT * FROM func_expell_student(42);
```

## ✔️ teacher/schedule/lesson
- metoda: POST
- body:
  - class_id::int,
  - course_id::int,
  - lesson_instance_id::int,
- output: 
```
{
	status: string,
	students: [
		student_id: number,
		first_name: string,
		last_name: string,
		attendance: string,
		grades: [
			grade_id: number,
			grade: number,
			grade_name: string,
			weight: number,
			lesson_date: string,
			lesson_id: number,
			description: string,
		]
	]
}
```
- przykładowe wykorzystanie funkcji z bazy danych:
```
// Tutaj dostaniemy listę uczniów z danej klasy
SELECT * FROM func_class_list(1); 

// Tą funkcją pobieramy wszystkie oceny dla studenta o student_id=21 z kursu o course_id=1
SELECT * FROM func_student_grades_from_course(21, 1);

// Ta funkcja zwróci boola czy student o student_id=17 był obecny lub nie,
// albo NULL'a, jak nie ma nic wpisanego na lesson_instance od lesson_instance_id=10
SELECT * FROM func_lesson_student_attendance(17, 10);
```

## ✔️ grades/add
- metoda: POST
- body:
  - lesson_instance_id::int,
  - student_id::int,
  - grade::int,
  - grade_type::int,
  - weight::int,
  - description::varchar(40)
- output: 
```
{
	success: boolean,
	message: string,
}
```
- przykładowe wykorzystanie funkcji z bazy:
```
SELECT * FROM func_add_grade(30, 21, 4, 3, 2, 'Odpowiedź z równań kwadratowych');
```

## ✔️ grades/remove
- metoda: POST
- body:
  - grade_id::int,
- output: 
```
{
	// -1 = >Grade of id % does not exist
	success: boolean,
	message: string,
}
```
- przykładowe wykorzystanie funkcji z bazy:
```
SELECT * FROM func_delete_grade(91);
```

## ✔️ grades/edit
- metoda: POST
- body:
  - grade_id::int,
  - lesson_instance_id::int,
  - grade_value::int,
  - grade_type::int,
  - weight::int,
  - description::varchar(40)
- output: 
```
{
	success: boolean,
	message: string,
}
```
- przykładowe wykorzystanie funkcji z bazy danych:
```
SELECT * FROM func_edit_grade(92, 30, 2, 3, 2, 'Odpowiedź z równań kwadratowych');
```
## ✔️ student/attendance/add
- metoda: POST
- body:
  - lesson_instance_id::int,
  - student_id::int
  - wasPresent::bool
- output: 
```
{
	success: boolean,
	message: string,
}
```
- przykładowe wykorzystanie funkcji z bazy danych:
```
// Tej funkcji użyjemy zawsze, dodaje do tabeli attendance
SELECT * FROM func_add_attendance(7, 11);

// Jeśli uczeń nie będzie obecny, dodajemy go do nieobecnych
SELECT * FROM func_add_absence(7, 11);
```

