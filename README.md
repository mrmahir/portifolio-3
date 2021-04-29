DROP TABLE IF EXISTS StudentInfo;
DROP TABLE IF EXISTS Courses;
DROP TABLE IF EXISTS Grades;

CREATE TABLE IF NOT EXISTS StudentInfo(
STUID TEXT NOT NULL PRIMARY KEY,
StudentNavn TEXT
);

CREATE TABLE IF NOT EXISTS Courses(
    COUID TEXT PRIMARY KEY,
    Course TEXT

);

CREATE TABLE IF NOT EXISTS Grades(
    Grades FLOAT,
    CoursesID TEXT,
    StudentID TEXT,
    PRIMARY KEY (Grades,CoursesID,StudentID),
    FOREIGN KEY (CoursesID) REFERENCES Courses (COUID)  ON UPDATE CASCADE ON DELETE RESTRICT,
    FOREIGN KEY (StudentID) REFERENCES StudentInfo (STUID)  ON UPDATE CASCADE ON DELETE RESTRICT
);

INSERT INTO Courses(COUID, Course) VALUES ('SD-2019', 'Software Development'),
                                        ('SD-2020', 'Software Development'),
                                         ('ES1-2019', 'Essential Computing 1');
INSERT INTO Grades(StudentID, CoursesID, Grades)VALUES ('AL', 'SD-2019',  12),
                                         ('AL', 'ES1-2019', 10),
                                         ('AN', 'SD-2020',null),
                                         ('AN', 'ES1-2019', 12),
                                         ('AJ', 'SD-2019',   7),
                                         ('AJ', 'ES1-2019', 10),
                                         ('BB', 'SD-2020',null),
                                         ('BB', 'ES1-2019',  2),
                                         ('AA', 'SD-2019',  10),
                                         ('AA', 'ES1-2019',  7),
                                         ('EE', 'SD-2020',null),
                                         ('EE', 'ES1-2019', 10),
                                         ('OO', 'SD-2019',   4),
                                         ('OO', 'ES1-2019', 12),
                                         ('SS', 'SD-2020',null),
                                         ('SS', 'ES1-2019', 12),
                                         ('TT', 'SD-2019',  12),
                                         ('TT', 'ES1-2019', 12),
                                         ('JJ', 'SD-2020',null),
                                         ('JJ', 'ES1-2019',  7);

INSERT INTO StudentInfo(STUID, StudentNavn) VALUES ('AL', 'Aisha Lincoln'),
                                                ('AN', 'Anya Nielsen'),
                                                ('AJ', 'Alfred Jensen'),
                                                ('BB', 'Berta Bertelsen'),
                                                ('AA', 'Albert Antonsen'),
                                                ('EE', 'Eske Eriksen'),
                                                ('OO', 'Olaf Olesen'),
                                                ('SS', 'Salma Simonsen'),
                                                ('TT', 'Theis Thomasen'),
                                                ('JJ', 'Janet Jensen');

--Average Grades
SELECT AVG(Grades)
FROM Grades;

--Average Grade for every student
SELECT StudentInfo.STUID, AVG(Grades.Grades) AS AverageGrades
FROM StudentInfo LEFT JOIN Grades ON StudentInfo.STUID= Grades.StudentID
GROUP BY StudentInfo.STUID;

--Average Grade for courses
SELECT Courses.COUID, AVG(Grades.Grades) AS AverageCourseGrade
FROM Courses LEFT JOIN Grades ON Courses.COUID = Grades.CoursesID
GROUP BY Courses.COUID;

--Course and Students grade
SELECT S1.StudentNavn, D1.StudentID, D1.Grades
FROM StudentInfo AS S1 JOIN Grades AS D1 ON D1.StudentID = D1.StudentID "
WHERE STUID = ?;

--Grade for selected course
SELECT  C1.Course, avg(S1.Grades)
FROM Courses AS C1 JOIN Grades AS S1 ON C1.COUID = S1.CoursesID
WHERE S1.CoursesID = ?;

-- Student and course together
SELECT S1.StudentNavn, C1.Course, G1.Grades
FROM Courses AS C1 JOIN Grades AS G1 ON C1.COUID = G1.CoursesID
JOIN StudentInfo AS S1 ON G1.StudentID = S1.STUID
WHERE G1.StudentID = ? AND G1.CoursesID = ?;
