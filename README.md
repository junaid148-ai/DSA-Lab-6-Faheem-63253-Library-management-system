#include <iostream>
#include <string>
using namespace std;

// Maximum limits
const int MAX_BOOKS = 100;
const int MAX_STACK = 100;

// Book structure
struct Book {
    int id;
    string title;
    string author;
};

// Global variables
Book books[MAX_BOOKS];
int bookCount = 0;

// Stack for deleted IDs
int deletedStack[MAX_STACK];
int stackTop = -1;

// Function to add book
void addBook() {
    if (bookCount >= MAX_BOOKS) {
        cout << "Library is full!\n";
        return;
    }
    
    Book* newBook = &books[bookCount];  // Using pointer
    
    cout << "\nEnter Book ID: ";
    cin >> newBook->id;
    cin.ignore();
    
    cout << "Enter Book Title: ";
    getline(cin, newBook->title);
    
    cout << "Enter Author Name: ";
    getline(cin, newBook->author);
    
    bookCount++;
    cout << "Book added successfully!\n";
}

// Function to search book (Linear Search)
int searchBook(int id) {
    for (int i = 0; i < bookCount; i++) {
        if (books[i].id == id) {
            return i;
        }
    }
    return -1;
}

// Function to delete book
void deleteBook() {
    if (bookCount == 0) {
        cout << "\nNo books to delete!\n";
        return;
    }
    
    int id;
    cout << "\nEnter Book ID to delete: ";
    cin >> id;
    
    int index = searchBook(id);
    
    if (index == -1) {
        cout << "Book not found!\n";
        return;
    }
    
    // Push to stack
    stackTop++;
    deletedStack[stackTop] = books[index].id;
    cout << "Book ID " << books[index].id << " saved for undo.\n";
    
    // Shift books left
    for (int i = index; i < bookCount - 1; i++) {
        books[i] = books[i + 1];
    }
    bookCount--;
    
    cout << "Book deleted successfully!\n";
}

// Function to undo delete (Stack Pop)
void undoDelete() {
    if (stackTop == -1) {
        cout << "\nNo deleted books to restore!\n";
        return;
    }
    
    if (bookCount >= MAX_BOOKS) {
        cout << "Library is full!\n";
        return;
    }
    
    // Pop from stack
    int restoredId = deletedStack[stackTop];
    stackTop--;
    
    Book* restoredBook = &books[bookCount];  // Using pointer
    restoredBook->id = restoredId;
    restoredBook->title = "[Restored]";
    restoredBook->author = "[Restored]";
    
    bookCount++;
    cout << "Book ID " << restoredId << " restored!\n";
}

// Function to search and display book
void searchAndDisplay() {
    if (bookCount == 0) {
        cout << "\nNo books in library!\n";
        return;
    }
    
    int id;
    cout << "\nEnter Book ID to search: ";
    cin >> id;
    
    int index = searchBook(id);
    
    if (index == -1) {
        cout << "Book not found!\n";
        return;
    }
    
    Book* found = &books[index];  // Using pointer
    cout << "\n--- Book Found ---\n";
    cout << "ID: " << found->id << endl;
    cout << "Title: " << found->title << endl;
    cout << "Author: " << found->author << endl;
}

// Function to display all books using 2D array
void displayAllBooks() {
    if (bookCount == 0) {
        cout << "\nNo books in library!\n";
        return;
    }
    
    // Create 2D array
    string table[MAX_BOOKS][3];
    
    // Fill 2D array
    for (int i = 0; i < bookCount; i++) {
        table[i][0] = to_string(books[i].id);
        table[i][1] = books[i].title;
        table[i][2] = books[i].author;
    }
    
    // Display table
    cout << "\n--- All Books ---\n";
    cout << "ID\t\tTitle\t\t\tAuthor\n";
    cout << "================================================\n";
    
    for (int i = 0; i < bookCount; i++) {
        cout << table[i][0] << "\t\t" 
             << table[i][1] << "\t\t" 
             << table[i][2] << endl;
    }
    
    cout << "================================================\n";
    cout << "Total Books: " << bookCount << endl;
}

// Main function
int main() {
    int choice;
    
    cout << "Welcome to Library Manager!\n";
    
    while (true) {
        cout << "--- MENU ---"<<endl;
        cout << "1. Add Book";
        cout << "2. Delete Book\n";
        cout << "3. Undo Delete (Stack Pop)\n";
        cout << "4. Search Book\n";
        cout << "5. Display All Books\n";
        cout << "6. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;
        
        if (choice == 1) {
            addBook();
        }
        else if (choice == 2) {
            deleteBook();
        }
        else if (choice == 3) {
            undoDelete();
        }
        else if (choice == 4) {
            searchAndDisplay();
        }
        else if (choice == 5) {
            displayAllBooks();
        }
        else if (choice == 6) {
            cout << "Thank you!\n";
            break;
        }
        else {
            cout << "Invalid choice!\n";
        }
    }
    
    return 0;
}
