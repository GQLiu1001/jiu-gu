```sql
CREATE DATABASE IF NOT EXISTS practice;

USE practice;

CREATE TABLE Students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(50) NOT NULL,
    major VARCHAR(50),
    enrollment_date DATE
);

CREATE TABLE Courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100) NOT NULL UNIQUE,
    department VARCHAR(50),
    credits INT
);

CREATE TABLE Enrollments (
    enrollment_id INT PRIMARY KEY AUTO_INCREMENT,
    student_id INT,
    course_id INT,
    grade DECIMAL(3,1) NULL,
    FOREIGN KEY (student_id) REFERENCES Students(student_id),
    FOREIGN KEY (course_id) REFERENCES Courses(course_id)
);

-- 向 Students 表插入数据
INSERT INTO Students (student_id, student_name, major, enrollment_date) VALUES
(1, '张三', '计算机科学', '2021-09-01'),
(2, '李四', '软件工程', '2022-09-01'),
(3, '王五', '计算机科学', '2021-09-01'),
(4, '赵六', '数学', '2023-09-01'),
(5, '孙七', '软件工程', '2022-09-01');

-- 向 Courses 表插入数据
INSERT INTO Courses (course_id, course_name, department, credits) VALUES
(101, '数据库原理', '计算机科学', 3),
(102, '操作系统', '计算机科学', 4),
(103, '高等数学', '数学', 5),
(104, 'Java程序设计', '软件工程', 3),
(105, '数据结构', '计算机科学', 4);

-- 向 Enrollments 表插入数据
INSERT INTO Enrollments (student_id, course_id, grade) VALUES
(1, 101, 3.7),
(1, 102, 3.0),
(1, 105, 4.0),
(2, 101, NULL), -- 李四选了数据库原理，但成绩未出
(2, 104, 3.3),
(3, 101, 3.3),
(3, 102, 3.7),
(5, 104, 2.7),
(5, 105, 3.0);
-- 赵六 (student_id 4) 没有选任何课
```