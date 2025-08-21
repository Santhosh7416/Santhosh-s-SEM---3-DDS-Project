## PROJECT 1: PERSONAL FINANCE TRACKER 
 **FEATURES** :
 1. Add new income/expenses records
 2. filter expenses (eg.above RS.100)
 3. sort by date or amount
 4. save & load data from a file
 5. Bonus: ASCII bar chart for monthly spending

 **Data Structure Used:** 
1. struct for transactions
2. array or vector of structs
3. File I/O using fstream
4. Optional: Data Structures Used:

**C++ Code:**


#include <iostream>
#include <vector>
#include <fstream>
#include <iomanip>
#include <algorithm>
using namespace std;

struct Transaction {
    string date;       
    string type;       
    string category;
    float amount;
};

vector<Transaction> transactions;

// Function declarations
void addTransaction();
void viewTransactions();
void saveToFile();
void loadFromFile();
void filterExpenses(float minAmount);
void showBarChart();
bool compareByAmount(Transaction a, Transaction b);

int main() {
    int choice;
    loadFromFile();

    do {
        cout << "\n========== Personal Finance Tracker ==========\n";
        cout << "1. Add Transaction\n";
        cout << "2. View All Transactions\n";
        cout << "3. Filter Expenses Over Amount\n";
        cout << "4. Show Spending Chart\n";
        cout << "5. Save and Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch(choice) {
            case 1: addTransaction(); break;
            case 2: viewTransactions(); break;
            case 3: {
                float amt;
                cout << "Enter minimum expense amount: ₹";
                cin >> amt;
                filterExpenses(amt);
                break;
            }
            case 4: showBarChart(); break;
            case 5: saveToFile(); cout << "Data saved. Exiting...\n"; break;
            default: cout << "Invalid choice.\n";
        }
    } while(choice != 5);

    return 0;
}

// Add new transaction
void addTransaction() {
    Transaction t;
    cout << "Enter date (YYYY-MM-DD): ";
    cin >> t.date;
    cout << "Enter type (Income/Expense): ";
    cin >> t.type;
    cout << "Enter category (e.g., Food, Rent): ";
    cin >> t.category;
    cout << "Enter amount: ₹";
    cin >> t.amount;
    transactions.push_back(t);
    cout << "Transaction added successfully.\n";
}

// Display all transactions
void viewTransactions() {
    if (transactions.empty()) {
        cout << "No transactions available.\n";
        return;
    }

    cout << "\n--- All Transactions ---\n";
    cout << left << setw(12) << "Date" 
         << setw(10) << "Type" 
         << setw(15) << "Category" 
         << setw(10) << "Amount" << "\n";
    cout << "-----------------------------------------------\n";

    for (auto &t : transactions) {
        cout << setw(12) << t.date 
             << setw(10) << t.type 
             << setw(15) << t.category 
             << "₹" << t.amount << "\n";
    }
}

// Save transactions to file
void saveToFile() {
    ofstream out("transactions.txt");
    for (auto &t : transactions) {
        out << t.date << " " << t.type << " " << t.category << " " << t.amount << "\n";
    }
    out.close();
}

// Load transactions from file
void loadFromFile() {
    ifstream in("transactions.txt");
    Transaction t;
    while (in >> t.date >> t.type >> t.category >> t.amount) {
        transactions.push_back(t);
    }
    in.close();
}

// Filter expenses above a certain amount
void filterExpenses(float minAmount) {
    cout << "\n--- Expenses above ₹" << minAmount << " ---\n";
    for (auto &t : transactions) {
        if (t.type == "Expense" && t.amount > minAmount) {
            cout << t.date << " | " << t.category << " | ₹" << t.amount << "\n";
        }
    }
}

// Optional: ASCII bar chart of expenses
void showBarChart() {
    cout << "\n--- Spending Chart (Expenses Only) ---\n";
    for (auto &t : transactions) {
        if (t.type == "Expense") {
            cout << setw(12) << t.category << " : ";
            int bars = static_cast<int>(t.amount / 10); // 1 bar per ₹10
            for (int i = 0; i < bars; i++) cout << "#";
            cout << " ₹" << t.amount << "\n";
        }
    }
}





