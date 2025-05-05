# CodeAlpha_CGPA_Calculator
This C++ program is a CGPA (Cumulative Grade Point Average) calculator designed to help students track their academic progress across semesters.

Key Features:
	•	User Input:
	•	Current semester number
	•	Previous CGPA and total credits
	•	Number of courses for the current semester
	•	For each course: course name, credit hours, and letter grade
	•	Grade Conversion:
	•	Uses a helper function getGradePoint() to convert letter grades (A, B+, etc.) into numerical grade points.
	•	Validation:
	•	Ensures inputs like CGPA, credits, and grades are valid.
	•	Exits the program with appropriate messages if invalid data is entered.
	•	Calculation Logic:
	•	Computes the total grade points earned in the current semester.
	•	Calculates the semester GPA (currentGPA) and updated CGPA (cgpa) using cumulative data.
	•	Output:
	•	Displays each course with its corresponding credit, grade, and grade points.
	•	Displays the total semester credits, total grade points, semester GPA, and updated CGPA.

Usage Example:

A user enters:
	•	Previous CGPA: 3.50
	•	Previous credits: 45
	•	3 courses for the current semester with grades like A, B+, and C

The program then shows the semester GPA and new CGPA based on the input.











#include <iostream>
#include <vector>
#include <string>
#include <iomanip> // Required for setprecision
#include <limits> // Required for numeric_limits

// Function to calculate grade point based on grade
double getGradePoint(const std::string& grade) {
    if (grade == "A") return 4.0;
    if (grade == "A-") return 3.67;
    if (grade == "B+") return 3.33;
    if (grade == "B") return 3.00;
    if (grade == "B-") return 2.67;
    if (grade == "C+") return 2.33;
    if (grade == "C") return 2.00;
    if (grade == "C-") return 1.67;
    if (grade == "D+") return 1.33;
    if (grade == "D") return 1.0;
    if (grade == "F") return 0.0;
    return -1.0; // Invalid grade
}

int main() {
    int numCourses;
    int currentSemester;
    double previousCGPA;
    double previousTotalCredits;
    std::cout << "Enter the current semester: ";
    std::cin >> currentSemester;

    if (currentSemester < 1) {
        std::cout << "Invalid semester number. Exiting." << std::endl;
        return 1;
    }

    std::cout << "Enter your previous CGPA: ";
    std::cin >> previousCGPA;
    if (previousCGPA < 0.0 || previousCGPA > 4.0) {
        std::cout << "Invalid previous CGPA. Exiting." << std::endl;
        return 1;
    }

    std::cout << "Enter your previous total credits: ";
    std::cin >> previousTotalCredits;
    if (previousTotalCredits < 0) {
        std::cout << "Invalid previous total credits. Exiting." << std::endl;
        return 1;
    }


    std::cout << "Enter the number of courses for this semester: ";
    std::cin >> numCourses;

    if (numCourses <= 0) {
        std::cout << "Invalid number of courses. Exiting." << std::endl;
        return 1;
    }

    std::vector<std::string> courseNames(numCourses);
    std::vector<int> courseCredits(numCourses);
    std::vector<std::string> courseGrades(numCourses);

    // Input course information
    for (int i = 0; i < numCourses; ++i) {
        std::cout << "Enter information for course " << i + 1 << ":" << std::endl;
        std::cout << "Course Name: ";
        std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n'); // Clear buffer before reading a line
        std::getline(std::cin, courseNames[i]);
        std::cout << "Credits: ";
        std::cin >> courseCredits[i];
        if (courseCredits[i] <= 0)
        {
            std::cout << "Credits cannot be zero or negative. Exiting" << std::endl;
            return 1;
        }
        std::cout << "Grade (e.g., A, A-, B+, B, B-, C+, C, C-, D+, D, F): ";
        std::cin >> courseGrades[i];
        // convert to uppercase
        for (char& c : courseGrades[i])
        {
            c = toupper(c);
        }

        double gradePoint = getGradePoint(courseGrades[i]);
        if (gradePoint == -1.0) {
            std::cout << "Invalid grade entered for course " << courseNames[i] << ". Exiting." << std::endl;
            return 1;
        }
    }

    // Calculate GPA and CGPA
    double currentCredits = 0.0;
    double currentGradePoints = 0.0;

    std::cout << "\nCourse Grades for Semester " << currentSemester << ":" << std::endl;
    for (int i = 0; i < numCourses; ++i) {
        double gradePoint = getGradePoint(courseGrades[i]);
        double courseGradePoints = gradePoint * courseCredits[i];
        currentGradePoints += courseGradePoints;
        currentCredits += courseCredits[i];
        std::cout << "Course: " << courseNames[i]
            << ", Credits: " << courseCredits[i]
            << ", Grade: " << courseGrades[i]
            << ", Grade Points: " << std::fixed << std::setprecision(2) << courseGradePoints << std::endl;
    }

    if (currentCredits == 0) {
        std::cout << "No credits earned this semester.  GPA is undefined." << std::endl;
        return 1;
    }
    double currentGPA = currentGradePoints / currentCredits;
    double totalCredits = previousTotalCredits + currentCredits;
    double totalGradePoints = (previousCGPA * previousTotalCredits) + currentGradePoints;
    double cgpa = totalGradePoints / totalCredits;


    std::cout << "Total Credits for Semester " << currentSemester << ": " << std::fixed << std::setprecision(2) << currentCredits << std::endl;
    std::cout << "Total Grade Points for Semester " << currentSemester << ": " << std::fixed << std::setprecision(2) << currentGradePoints << std::endl;
    std::cout << "GPA for Semester " << currentSemester << ": " << std::fixed << std::setprecision(2) << currentGPA << std::endl;
    std::cout << "Current Semester: " << currentSemester << std::endl;
    std::cout << "Current CGPA: " << std::fixed << std::setprecision(2) << cgpa << std::endl;

    return 0;
}
