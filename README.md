# PROJECT: STUDENT PERFORMANCE ANALYSIS SYSTEM USING PYTHON

## 1. AIM

To develop a Python-based Student Performance Analysis System that manages student academic records, calculates marks and percentages, determines grades, identifies academic performance levels, and generates statistical reports using Python programming concepts.

## 2. PROBLEM STATEMENT

Managing student academic records manually can lead to calculation errors, inefficient data organization, and difficulties in identifying students who require academic improvement.

This project aims to automate the process of student performance evaluation by developing a centralized Python application that processes student information, calculates academic scores, assigns grades, and generates performance summaries.

## 4. ALGORITHM

**Step 1:** Start the program.

**Step 2:** Initialize an empty data structure to store student records.

**Step 3:** Display the main menu with the following options:
- Add student details.
- Display student records.
- Analyze student performance.
- Generate class statistics.
- Exit.

**Step 4:** Accept the user's choice.

**Step 5:** If the user selects Add Student:
- Enter the student's name and roll number.
- Enter marks for five subjects.
- Validate the entered marks.
- Calculate total marks and percentage.
- Determine the corresponding grade.
- Store the student information.

**Step 6:** If the user selects Display Records:
- Retrieve all stored student information.
- Display student details, marks, percentages and grades.

**Step 7:** If the user selects Performance Analysis:
- Calculate individual student performance.
- Identify strengths and weaknesses.
- Categorize students according to their grades.

**Step 8:** If the user selects Class Statistics:
- Calculate the class average.
- Identify the highest and lowest percentages.
- Count the number of students in each grade category.

**Step 9:** Display the generated academic report.

**Step 10:** Repeat the process until the user selects Exit.

**Step 11:** Stop the program.

## 5. PYTHON SOURCE CODE

```python
# ==============================================
# STUDENT PERFORMANCE ANALYSIS SYSTEM
# Developed using Python
# ==============================================

import statistics
from datetime import datetime


class Student:
    """Class to manage individual student records."""

    def __init__(self, roll_number, name, marks):

        self.roll_number = roll_number
        self.name = name
        self.marks = marks

        self.total = sum(marks.values())

        self.average = self.total / len(marks)

        self.percentage = round(self.average, 2)

        self.grade = self.calculate_grade()

    def calculate_grade(self):

        if self.percentage >= 90:
            return "A+"

        elif self.percentage >= 80:
            return "A"

        elif self.percentage >= 70:
            return "B"

        elif self.percentage >= 60:
            return "C"

        elif self.percentage >= 50:
            return "D"

        else:
            return "F"

    def get_performance(self):

        if self.percentage >= 90:
            return "Excellent Performance"

        elif self.percentage >= 75:
            return "Very Good Performance"

        elif self.percentage >= 60:
            return "Good Performance"

        elif self.percentage >= 50:
            return "Average Performance"

        else:
            return "Needs Improvement"

    def display(self):

        print("\n" + "=" * 50)

        print("Roll Number :", self.roll_number)
        print("Student Name:", self.name)

        print("\nSubject-wise Marks:")

        for subject, mark in self.marks.items():

            print(f"{subject:<15}: {mark}")

        print("\nTotal Marks :", self.total)
        print("Percentage  :", self.percentage, "%")
        print("Grade       :", self.grade)

        print("Performance :", self.get_performance())

        print("=" * 50)


class PerformanceAnalyzer:
    """Class to analyze class-level performance."""

    def __init__(self):

        self.students = []

    def add_student(self):

        print("\n--- ADD STUDENT DETAILS ---")

        roll_number = input("Enter Roll Number: ")

        if any(
            student.roll_number == roll_number
            for student in self.students
        ):

            print("Roll number already exists!")
            return

        name = input("Enter Student Name: ")

        subjects = [
            "Mathematics",
            "Physics",
            "Chemistry",
            "Computer Science",
            "English"
        ]

        marks = {}

        for subject in subjects:

            while True:

                try:

                    mark = float(
                        input(f"Enter {subject} Marks: ")
                    )

                    if 0 <= mark <= 100:

                        marks[subject] = mark
                        break

                    else:

                        print(
                            "Enter marks between 0 and 100."
                        )

                except ValueError:

                    print("Please enter a valid number.")

        student = Student(
            roll_number,
            name,
            marks
        )

        self.students.append(student)

        print("\nStudent Record Added Successfully!")

    def display_all_students(self):

        if not self.students:

            print("\nNo student records available.")
            return

        print("\n--- STUDENT RECORDS ---")

        for student in self.students:

            student.display()

    def search_student(self):

        roll = input(
            "\nEnter Roll Number to Search: "
        )

        for student in self.students:

            if student.roll_number == roll:

                student.display()
                return

        print("\nStudent Record Not Found!")

    def analyze_performance(self):

        if not self.students:

            print("\nNo records available for analysis.")
            return

        print("\n--- PERFORMANCE ANALYSIS ---")

        for student in self.students:

            print(
                f"\n{student.name} "
                f"({student.roll_number})"
            )

            print(
                "Percentage:",
                student.percentage,
                "%"
            )

            print("Grade:", student.grade)

            print(
                "Result:",
                student.get_performance()
            )

            weak_subjects = [
                subject
                for subject, mark in student.marks.items()
                if mark < 50
            ]

            if weak_subjects:

                print(
                    "Subjects Requiring Improvement:",
                    ", ".join(weak_subjects)
                )

            else:

                print("All subjects are above 50 marks.")

    def generate_statistics(self):

        if not self.students:

            print("\nNo records available.")
            return

        percentages = [
            student.percentage
            for student in self.students
        ]

        print("\n" + "=" * 55)

        print("          CLASS PERFORMANCE REPORT")

        print("=" * 55)

        print(
            "Total Students:",
            len(self.students)
        )

        print(
            "Class Average:",
            round(statistics.mean(percentages), 2),
            "%"
        )

        print(
            "Highest Percentage:",
            max(percentages),
            "%"
        )

        print(
            "Lowest Percentage:",
            min(percentages),
            "%"
        )

        print("\nGRADE DISTRIBUTION")

        grade_counts = {}

        for student in self.students:

            grade_counts[student.grade] = (
                grade_counts.get(student.grade, 0) + 1
            )

        for grade, count in sorted(grade_counts.items()):

            print(f"Grade {grade}: {count} student(s)")

        topper = max(
            self.students,
            key=lambda student: student.percentage
        )

        print("\nTOP PERFORMING STUDENT")

        print("Name:", topper.name)
        print("Roll Number:", topper.roll_number)
        print("Percentage:", topper.percentage, "%")

        print("=" * 55)

    def export_report(self):

        if not self.students:

            print("\nNo records available to export.")
            return

        filename = "student_performance_report.txt"

        with open(filename, "w", encoding="utf-8") as file:

            file.write(
                "STUDENT PERFORMANCE ANALYSIS REPORT\n"
            )

            file.write(
                f"Generated: {datetime.now()}\n\n"
            )

            for student in self.students:

                file.write(
                    f"Name: {student.name}\n"
                )

                file.write(
                    f"Roll Number: {student.roll_number}\n"
                )

                file.write(
                    f"Total Marks: {student.total}\n"
                )

                file.write(
                    f"Percentage: {student.percentage}%\n"
                )

                file.write(
                    f"Grade: {student.grade}\n"
                )

                file.write(
                    f"Performance: {student.get_performance()}\n"
                )

                file.write("-" * 40 + "\n")

        print(
            "\nReport generated successfully:",
            filename
        )

    def run(self):

        while True:

            print("\n")
            print("*" * 55)

            print(
                "       STUDENT PERFORMANCE ANALYSIS SYSTEM"
            )

            print("*" * 55)

            print("1. Add Student")
            print("2. Display All Students")
            print("3. Search Student")
            print("4. Analyze Performance")
            print("5. Generate Class Statistics")
            print("6. Export Performance Report")
            print("7. Exit")

            print("*" * 55)

            choice = input(
                "\nEnter Your Choice (1-7): "
            )

            if choice == "1":

                self.add_student()

            elif choice == "2":

                self.display_all_students()

            elif choice == "3":

                self.search_student()

            elif choice == "4":

                self.analyze_performance()

            elif choice == "5":

                self.generate_statistics()

            elif choice == "6":

                self.export_report()

            elif choice == "7":

                print(
                    "\nThank you for using the system!"
                )

                break

            else:

                print(
                    "\nInvalid Choice! Try Again."
                )


# ==============================================
# MAIN PROGRAM
# ==============================================

if __name__ == "__main__":

    system = PerformanceAnalyzer()

    system.run()
```

## 6. SAMPLE INPUT

```text
Enter Your Choice: 1

Enter Roll Number: 101
Enter Student Name: Arun

Enter Mathematics Marks: 95
Enter Physics Marks: 88
Enter Chemistry Marks: 92
Enter Computer Science Marks: 96
Enter English Marks: 89

Student Record Added Successfully!
```

## 7. SAMPLE OUTPUT

```text
*******************************************************
       STUDENT PERFORMANCE ANALYSIS SYSTEM
*******************************************************

1. Add Student
2. Display All Students
3. Search Student
4. Analyze Performance
5. Generate Class Statistics
6. Export Performance Report
7. Exit

Enter Your Choice: 2

==================================================
Roll Number : 101
Student Name: Arun

Subject-wise Marks:

Mathematics    : 95
Physics        : 88
Chemistry      : 92
Computer Science: 96
English        : 89

Total Marks : 460
Percentage  : 92.0 %
Grade       : A+
Performance : Excellent Performance

==================================================
```

### Class Performance Report

```text
=======================================================
          CLASS PERFORMANCE REPORT
=======================================================

Total Students: 3

Class Average: 81.33 %

Highest Percentage: 92.0 %

Lowest Percentage: 65.0 %

GRADE DISTRIBUTION

Grade A+: 1 student(s)
Grade A: 1 student(s)
Grade C: 1 student(s)

TOP PERFORMING STUDENT

Name: Arun
Roll Number: 101
Percentage: 92.0 %

=======================================================
```

## 8. RESULT

Thus, the Student Performance Analysis System was successfully developed using Python programming.

The application efficiently manages student academic records, calculates total marks and percentages, automatically assigns grades, analyzes individual performance, identifies subjects requiring improvement, and generates class-level statistical reports.

The project demonstrates the practical implementation of object-oriented programming, classes, objects, functions, loops, conditional statements, exception handling, lists, dictionaries and file handling in Python.

