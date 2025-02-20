#include <iostream>
#include <fstream>
#include <vector>
#include <iomanip>

using namespace std;

struct Expense {
    string category;
    double amount;
};

void addExpense(vector<Expense>& expenses) {
    Expense newExpense;
    cout << "Enter category: ";
    cin >> newExpense.category;
    cout << "Enter amount: ";
    cin >> newExpense.amount;
    expenses.push_back(newExpense);
    cout << "Expense added successfully!\n";
}

void viewExpenses(const vector<Expense>& expenses) {
    if (expenses.empty()) {
        cout << "No expenses recorded yet.\n";
        return;
    }
    cout << "\n--- Expense List ---\n";
    cout << left << setw(15) << "Category" << setw(10) << "Amount" << endl;
    cout << "---------------------\n";
    for (const auto& expense : expenses) {
        cout << left << setw(15) << expense.category << setw(10) << expense.amount << endl;
    }
}

void calculateTotal(const vector<Expense>& expenses) {
    double total = 0;
    for (const auto& expense : expenses) {
        total += expense.amount;
    }
    cout << "Total Expenses: Rs. " << total << "\n";
}

void saveExpenses(const vector<Expense>& expenses, const string& filename) {
    ofstream file(filename);
    if (!file) {
        cout << "Error saving expenses!\n";
        return;
    }
    for (const auto& expense : expenses) {
        file << expense.category << " " << expense.amount << "\n";
    }
    file.close();
    cout << "Expenses saved successfully!\n";
}

void loadExpenses(vector<Expense>& expenses, const string& filename) {
    ifstream file(filename);
    if (!file) {
        cout << "No previous expenses found.\n";
        return;
    }
    Expense expense;
    while (file >> expense.category >> expense.amount) {
        expenses.push_back(expense);
    }
    file.close();
    cout << "Expenses loaded successfully!\n";
}

int main() {
    vector<Expense> expenses;
    string filename = "expenses.txt";
    loadExpenses(expenses, filename);
    
    int choice;
    do {
        cout << "\nExpense Tracker\n";
        cout << "1. Add Expense\n";
        cout << "2. View Expenses\n";
        cout << "3. Calculate Total\n";
        cout << "4. Save Expenses\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;
        
        switch (choice) {
            case 1:
                addExpense(expenses);
                break;
            case 2:
                viewExpenses(expenses);
                break;
            case 3:
                calculateTotal(expenses);
                break;
            case 4:
                saveExpenses(expenses, filename);
                break;
            case 5:
                cout << "Exiting...\n";
                break;
            default:
                cout << "Invalid choice! Try again.\n";
        }
    } while (choice != 5);
    
    return 0;
}

