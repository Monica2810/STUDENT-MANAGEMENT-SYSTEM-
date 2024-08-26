# STUDENT-MANAGEMENT-SYSTEM-

A simple Python-based system to manage students' data. This program allows users to add, update, delete, and view student records via a text-based menu system. The project follows SOLID principles, eliminates redundancy, and simplifies the design while avoiding unnecessary complexity.

The following is the refactored code: 

class Student:

    A class representing a student.
    """
    Attributes:
    id (int): The student's unique identifier.
    name (str): The student's name.
    age (int): The student's age.
    major (str): The student's major.
    """

    def __init__(self, id, name, age, major):
        """Initialize the student object with ID, name, age, and major."""
        self.id = id
        self.name = name
        self.age = age
        self.major = major

    def update(self, name=None, age=None, major=None):
        """
        Update student information.
        
        Parameters:
        name (str, optional): New name for the student.
        age (int, optional): New age for the student.
        major (str, optional): New major for the student.
        """
        if name:
            self.name = name
        if age:
            self.age = age
        if major:
            self.major = major

    def display(self):
        """Return a string representation of the student's details."""
        return f"ID: {self.id}, Name: {self.name}, Age: {self.age}, Major: {self.major}"


class StudentDatabase:
    """
    A class representing the student database.
    
    This class is responsible for managing the collection of students.
    
    Attributes:
    students (dict): A dictionary storing students with their ID as the key.
    """

    def __init__(self):
        """Initialize the student database with an empty dictionary."""
        self.students = {}

    def add(self, student):
        """
        Add a student to the database.
        
        Parameters:
        student (Student): The student object to be added.
        """
        self.students[student.id] = student

    def remove(self, student_id):
        """
        Remove a student from the database by ID.
        
        Parameters:
        student_id (int): The unique ID of the student to be removed.
        
        Returns:
        Student: The removed student, or None if no student with the given ID exists.
        """
        return self.students.pop(student_id, None)

    def get(self, student_id):
        """
        Retrieve a student from the database by ID.
        
        Parameters:
        student_id (int): The unique ID of the student to retrieve.
        
        Returns:
        Student: The student object, or None if no student with the given ID exists.
        """
        return self.students.get(student_id)

    def display_all(self):
        """
        Get a list of all students in the database.
        
        Returns:
        list: A list of strings representing all students.
        """
        return [student.display() for student in self.students.values()]


class StudentManagementSystem:
    """
    A class representing the student management system.
    
    This class interacts with the StudentDatabase to manage student operations.
    """

    def __init__(self):
        """Initialize the student management system with an empty database."""
        self.database = StudentDatabase()

    def add_student(self, id, name, age, major):
        """
        Add a new student to the system.
        
        Parameters:
        id (int): Student's unique ID.
        name (str): Student's name.
        age (int): Student's age.
        major (str): Student's major.
        
        Returns:
        str: Success or failure message.
        """
        if self.database.get(id):
            return "Student with this ID already exists."
        student = Student(id, name, age, major)
        self.database.add(student)
        return "Student added successfully."

    def delete_student(self, student_id):
        """
        Delete a student from the system by ID.
        
        Parameters:
        student_id (int): The ID of the student to be deleted.
        
        Returns:
        str: Success or failure message.
        """
        student = self.database.remove(student_id)
        if student:
            return "Student removed successfully."
        return "Student not found."

    def update_student(self, student_id, name=None, age=None, major=None):
        """
        Update student information.
        
        Parameters:
        student_id (int): The unique ID of the student to be updated.
        name (str, optional): New name for the student.
        age (int, optional): New age for the student.
        major (str, optional): New major for the student.
        
        Returns:
        str: Success or failure message.
        """
        student = self.database.get(student_id)
        if student:
            student.update(name, age, major)
            return "Student information updated successfully."
        return "Student not found."

    def list_students(self):
        """
        List all students in the system.
        
        Returns:
        str: A formatted list of all students or a message if no students exist.
        """
        students = self.database.display_all()
        if students:
            return "\n".join(students)
        return "No students found."


class MenuSystem:
    """
    A class representing a simple text-based menu for interacting with the student management system.
    
    This class allows users to perform various operations like adding, deleting, updating, and viewing students.
    """

    def __init__(self):
        """Initialize the menu system with a student management system instance."""
        self.system = StudentManagementSystem()

    def display_menu(self):
        """Display the menu options to the user."""
        print("\nStudent Management System")
        print("1. Add Student")
        print("2. Delete Student")
        print("3. Update Student")
        print("4. View All Students")
        print("5. Exit")

    def run(self):
        """Run the menu system, accepting user inputs and executing corresponding actions."""
        while True:
            self.display_menu()
            choice = input("Enter your choice: ")

            if choice == "1":
                self.add_student()
            elif choice == "2":
                self.delete_student()
            elif choice == "3":
                self.update_student()
            elif choice == "4":
                self.view_all_students()
            elif choice == "5":
                print("Exiting the system.")
                break
            else:
                print("Invalid choice. Please try again.")

    def add_student(self):
        """Prompt the user to add a new student to the system."""
        id = int(input("Enter Student ID: "))
        name = input("Enter Student Name: ")
        age = int(input("Enter Student Age: "))
        major = input("Enter Student Major: ")
        message = self.system.add_student(id, name, age, major)
        print(message)

    def delete_student(self):
        """Prompt the user to delete a student by ID."""
        student_id = int(input("Enter Student ID to delete: "))
        message = self.system.delete_student(student_id)
        print(message)

    def update_student(self):
        """Prompt the user to update a student's information."""
        student_id = int(input("Enter Student ID to update: "))
        name = input("Enter new name (leave blank to keep unchanged): ")
        age = input("Enter new age (leave blank to keep unchanged): ")
        major = input("Enter new major (leave blank to keep unchanged): ")

        age = int(age) if age else None
        message = self.system.update_student(student_id, name, age, major)
        print(message)

    def view_all_students(self):
        """Display all students in the system."""
        print(self.system.list_students())




# Features

-Add a new student with ID, name, age, and major.

-Delete a student by ID.
-Update a student's information (name, age, and major).

-View a list of all students.

-Simple text-based menu for user interaction.

# Installation and Running

1. Clone the repository to your local machine:
   https://github.com/Monica2810/STUDENT-MANAGEMENT-SYSTEM-.git

3. Navigate to the project directory:
student-management-system

4. Run the program:
   python main.py

# How to Use

When you run the program, you will be presented with a menu with the following options:

Add Student: Input the student's ID, name, age, and major.

Delete Student: Enter the ID of the student to delete.

Update Student: Enter the student's ID, and optionally update their name, age, and major.

View All Students: Display a list of all students in the system.

Exit: Exit the program

   # Refactoring Changes
   
The code was refactored with the following improvements or changes:

SOLID Principles: The program was redesigned to follow SOLID principles for maintainability and scalability.

DRY (Don't Repeat Yourself): Redundant code, like searching for students by ID, was eliminated.

KISS (Keep It Simple, Stupid): The program logic was simplified, avoiding unnecessary complexity.
YAGNI (You Ain't Gonna Need It): Features or functionality not required were avoided.

Dictionary-Based Storage: Student records are stored in a dictionary for faster lookups and updates by ID.
