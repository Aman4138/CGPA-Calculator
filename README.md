# C++
CGPA Calculator
#include <iostream>
#include <string>
#include <iomanip>
#include <vector>
#include <algorithm>
using namespace std;
class Subject {
private:
    string name;
    double caMarks;
    double mteMarks;
    double attendanceMarks;
    double endTermMarks;
public:
    Subject(string subjectName) : name(subjectName), caMarks(0.0), mteMarks(0.0), attendanceMarks(0.0), endTermMarks(0.0) {}
    string getName() const {
        return name;
    }
    void calculateCAMarks() {
        int numCAs;
        cout << "\nEnter how many CAs are in " << name << ": ";
        cin >> numCAs;
        vector<double> caMarksList(numCAs);
        for (int j = 0; j < numCAs; j++) {
            cout << j + 1;
            if (j + 1 == 1) cout << "st CA mark: ";
            else if (j + 1 == 2) cout << "nd CA mark: ";
            else if (j + 1 == 3) cout << "rd CA mark: ";
            else cout << "th CA mark: ";
            cin >> caMarksList[j];
        }
        sort(caMarksList.rbegin(), caMarksList.rend());
        int countToConsider = (numCAs == 3) ? 2 : min(numCAs, 3);
        double totalCA = 0.0;
        for (int j = 0; j < countToConsider; j++) {
            totalCA += caMarksList[j];
        }
        double weightage;
        cout << "Enter the weightage of CA marks for " << name << ": ";
        cin >> weightage;

        caMarks = (totalCA / (countToConsider * 30)) * weightage;
    }
    void calculateMTEMarks() {
        double totalQuestions, obtainedMarks, weightage;
        cout << "\nFor the subject " << name << ":\n";
        cout << "   Enter total questions in MTE: ";
        cin >> totalQuestions;
        cout << "   Enter marks obtained out of " << totalQuestions << ": ";
        cin >> obtainedMarks;
        cout << "   Enter the weightage of MTE: ";
        cin >> weightage;
        double percentage = (obtainedMarks / totalQuestions) * 100;
        mteMarks = (percentage / 100) * weightage;
    }
    void calculateAttendanceMarks() {
        double attendancePercentage, weightage;
        cout << "\nFor the subject " << name << ":\n";
        cout << "   Enter your attendance percentage (0-100): ";
        cin >> attendancePercentage;
        cout << "   Enter the weightage of attendance marks: ";
        cin >> weightage;
        attendanceMarks = (attendancePercentage / 100) * weightage;
    }
    void calculateEndTermMarks() {
        double totalMarks, obtainedMarks, weightage;
        cout << "\nFor the subject " << name << ":\n";
        cout << "   Enter total marks for the End-Term: ";
        cin >> totalMarks;
        cout << "   Enter the marks you obtained out of " << totalMarks << ": ";
        cin >> obtainedMarks;
        cout << "   Enter the weightage of the End-Term: ";
        cin >> weightage;
        double percentage = (obtainedMarks / totalMarks) * 100;
        endTermMarks = (percentage / 100) * weightage;
    }
    double getTotalWeightedMarks() const {
        return caMarks + mteMarks + attendanceMarks + endTermMarks;
    }
    void display() const {
        cout << setw(20) << name
             << setw(15) << fixed << setprecision(2) << caMarks
             << setw(15) << mteMarks
             << setw(20) << attendanceMarks
             << setw(15) << endTermMarks
             << setw(20) << getTotalWeightedMarks() << endl;
    }
};
double calculateCGPA(double percentage) {
    return percentage / 10; // Standard conversion: Percentage ÷ 10
}
int main() {
    string name;
    int subCount;
    cout << "!----------   Please Enter Your Name : ----------!" << endl;
    getline(cin, name);
    cout << "\n     **----------- THANKS FOR VISITING, " << name << " --------------**" << endl;
    cout << "\nDear, " << name << ", how many subjects do you have? ";
    cin >> subCount;
    vector<Subject> subjects;
    cin.ignore(); // Clear input buffer
    for (int i = 0; i < subCount; i++) {
        cout << "\nEnter the name of subject " << (i + 1) << ": ";
        string subjectName;
        getline(cin, subjectName);
        subjects.emplace_back(subjectName);
    }
    for (auto& subject : subjects) {
        subject.calculateCAMarks();
        subject.calculateMTEMarks();
        subject.calculateAttendanceMarks();
        subject.calculateEndTermMarks();
    }
    double totalWeightedMarks = 0.0;
    for (const auto& subject : subjects) {
        totalWeightedMarks += subject.getTotalWeightedMarks();
    }
    double percentage = (totalWeightedMarks / (subCount * 100)) * 100; // Assuming each subject is out of 100 marks
    double cgpa = calculateCGPA(percentage);
    cout << "\n\n--- Final Summary ---\n";
    cout << setw(20) << "Subject"
         << setw(15) << "CA Marks"
         << setw(15) << "MTE Marks"
         << setw(20) << "Attendance Marks"
         << setw(15) << "End-Term"
         << setw(20) << "Total Weighted" << endl;
    cout << string(100, '-') << endl;
    for (const auto& subject : subjects) {
        subject.display();
    }
    cout << "\nTotal Weighted Marks: " << totalWeightedMarks << endl;
    cout << "Percentage: " << fixed << setprecision(2) << percentage << "%" << endl;
    cout << "CGPA: " << fixed << setprecision(2) << cgpa << endl;
    return 0;
}
