//in the initialization used array to store the fixed values if you dont wanna 
//use static , create it dynamically using pointers.

// u can identify the operations whatever u entered in the ppt . if anything missed
//LMK  i'll add it.

// fgets -> used to get string input like a sentence or paragraph(include spaces)

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct Book {
    char title[100];          
    char author[100];         
    int publicationYear;     
    int isAvailable;         
    int catalogNumber;       
} Book;
typedef struct Member {
    char name[100];           
    char email[100];          
    int membershipID;        
} Member;
typedef struct Transaction {
    int bookCatalogNumber;   
    int memberID;            
    char transactionType[100];
} Transaction;
typedef struct Library {
    Book books[100];         
    int numBooks;            
    Member members[100];     
    int numMembers;          
    Transaction transactions[100]; 
    int numTransactions;
} Library;

void addBook(Library* library) {
    if (library->numBooks >= 100) {
        printf("==============================================\n");
        printf("we dont have  enough space to keep your book...\n");
        printf("==============================================\n");
        return;
    }
    Book newBook;
    printf("Enter the Title: ");
    getchar(); 
    fgets(newBook.title, 50, stdin);
    newBook.title[strcspn(newBook.title, "\n")] = 0; 
    printf("Enter the Author: ");
    fgets(newBook.author, 50, stdin);
    newBook.author[strcspn(newBook.author, "\n")] = 0;
    printf("Enter the Publication Year: ");
    scanf("%d", &newBook.publicationYear);
    newBook.isAvailable = 1;
    newBook.catalogNumber = library->numBooks + 1; 

    library->books[library->numBooks] = newBook;
    library->numBooks++;
    printf("==========================================\n");
    printf("BOOK ADDED SUCCESSFULLY.\n");
    printf("==========================================\n");
}
void displayBooks(Library* library) {
    if (library->numBooks == 0) {
        printf("NO BOOKS.\n");
        return;
    }
    printf("==========================================\n");
    printf("BOOKS IN LIBRARY :\n");
    for (int i = 0; i < library->numBooks; i++) {
        printf("Catalog Number: %d\n", library->books[i].catalogNumber);
        printf("Title: %s\n", library->books[i].title);
        printf("Author: %s\n", library->books[i].author);
        printf("Publication Year: %d\n", library->books[i].publicationYear);
        printf("Availability: %s\n", library->books[i].isAvailable ? "Yes" : "No");
        
        printf("\n");
        
    }
    printf("==========================================\n");
}
void searchBookByTitle(Library* library) {
    char title[50];
    printf("Enter the titl : ");
    getchar(); 
    fgets(title, 50, stdin);
    title[strcspn(title, "\n")] = 0; 
    int found = 0;
    for (int i = 0; i < library->numBooks; i++) {
        if (strcmp(library->books[i].title, title) == 0) {
            printf("==========================================\n");
            printf("BOOK DETAILS :\n");
            printf("Catalog Number: %d\n", library->books[i].catalogNumber);
            printf("Title: %s\n", library->books[i].title);
            printf("Author: %s\n", library->books[i].author);
            printf("Publication Year: %d\n", library->books[i].publicationYear);
            printf("Availability: %s\n", library->books[i].isAvailable ? "Yes" : "No");
            printf("==========================================\n");
            found = 1;
            break;
        }
    }
    if (!found) {
        printf("==========================================\n");
        printf("BOOK NOT FOUND.\n");
        printf("==========================================\n");
    }
}
void registerMember(Library* library) {
    if (library->numMembers >= 100) {
        printf("Memeber ship is over\n");
        return;
    }
    Member newMember;
    printf("Enter the member name: ");
    getchar(); 
    fgets(newMember.name, 50, stdin);
    newMember.name[strcspn(newMember.name, "\n")] = 0; 
    printf("Enter the member email: ");
    fgets(newMember.email, 50, stdin);
    newMember.email[strcspn(newMember.email, "\n")] = 0;
    newMember.membershipID = library->numMembers + 1;

    library->members[library->numMembers] = newMember;
    library->numMembers++;
    printf("==========================================\n");
    printf("MEMBER REGISTERED SUCCESSFULLY.\n");
    printf("==========================================\n");
}
void displayMembers(Library* library) {
    if (library->numMembers == 0) {
        printf("No members are registered at the moment.\n");
        return;
    }
    printf("==========================================\n");
    printf("REGISTERED MEMBERS:\n");
    for (int i = 0; i < library->numMembers; i++) {
        printf("Membership ID: %d\n", library->members[i].membershipID);
        printf("Name: %s\n", library->members[i].name);
        printf("Email: %s\n", library->members[i].email);
        printf("\n");
    }
    printf("==========================================\n");
}
void borrowBook(Library* library) {
    int catalogNumber, memberID;

    printf("Enter the catalog number: ");
    scanf("%d", &catalogNumber);
    printf("Enter your membership ID: ");
    scanf("%d", &memberID);

    int bookIndex = -1;
    for (int i = 0; i < library->numBooks; i++) {
        if (library->books[i].catalogNumber == catalogNumber) {
            bookIndex = i;
            break;
        }
    }
    if (bookIndex == -1 || !library->books[bookIndex].isAvailable) {
        printf("========================================================\n");
        printf("Book is not available here, please go any other library.\n");
        printf("========================================================\n");
        return;
    }
    int memberIndex = -1;
    for (int i = 0; i < library->numMembers; i++) {
        if (library->members[i].membershipID == memberID) {
            memberIndex = i;
            break;
        }
    }
    if (memberIndex == -1) {
        printf("=====================================================\n");
        printf("Create the memberID and come to library...GET OUT\n");
        printf("=====================================================\n");
        return;
    }
      library->books[bookIndex].isAvailable = 0;
    library->transactions[library->numTransactions].bookCatalogNumber = catalogNumber;
    library->transactions[library->numTransactions].memberID = memberID;
    strcpy(library->transactions[library->numTransactions].transactionType, "Borrow");
    library->numTransactions++;
    printf("==========================================\n");
    printf("BOOK BORROWED SUCCESSFULLY.\n");
    printf("==========================================\n");
}
void returnBook(Library* library) {
    int catalogNumber, memberID;
    printf("Enter the catalog numbe: ");
    scanf("%d", &catalogNumber);
    printf("Enter your membership ID: ");
    scanf("%d", &memberID);
    int bookIndex = -1;
    for (int i = 0; i < library->numBooks; i++) {
        if (library->books[i].catalogNumber == catalogNumber) {
            bookIndex = i;
            break;
        }
    }
    if (bookIndex == -1 || library->books[bookIndex].isAvailable) {
        printf("Sorry\n");
        return;
    }
    int memberIndex = -1;
    for (int i = 0; i < library->numMembers; i++) {
        if (library->members[i].membershipID == memberID) {
            memberIndex = i;
            break;
        }
    }
    if (memberIndex == -1) {
        printf("member ID is invalid.\n");
        return;
    }
    library->books[bookIndex].isAvailable = 1;
    library->transactions[library->numTransactions].bookCatalogNumber = catalogNumber;
    library->transactions[library->numTransactions].memberID = memberID;
    strcpy(library->transactions[library->numTransactions].transactionType, "Return");
    library->numTransactions++;
    printf("==========================================\n");
    printf("BOOK RETURNED SUCCESSFULLY.\n");
    printf("==========================================\n");
}

void displayTransactions(Library* library) {
    if (library->numTransactions == 0) {
        printf("No transactions have been recorded .\n");
        return;
    }
    printf("==========================================\n");
    printf("TRANSACTION DETAILS :\n");
    for (int i = 0; i < library->numTransactions; i++) {
        printf("Transaction Type: %s\n", library->transactions[i].transactionType);
        printf("Book Catalog Number: %d\n", library->transactions[i].bookCatalogNumber);
        printf("Member ID: %d\n", library->transactions[i].memberID);
        printf("\n");
    }
    printf("==========================================\n");
}

int main() {
    Library library = {0}; 
    printf("*********************************************\n");
    printf("*       $ WELCOME TO OUR LIBRARY $          *\n");
    printf("*********************************************\n");
    printf("*           PRESENTED by                    *\n");
    printf("*               * HARSHITHA                 *\n");
    printf("*               * VENNELA                   *\n");
    printf("*               * YASWITHA                  *\n");
    printf("*                                           *\n");
    printf("*********************************************\n");
    while (1) {
        printf("1.Add a Book\n");
        printf("2.Display Books\n");
        printf("3.Search for a Book \n");
        printf("4.Register a Member\n");
        printf("5.Display All Members\n");
        printf("6.Borrow a Book\n");
        printf("7.Return a Book\n");
        printf("8.Display Transactions\n");
        printf("9.Exit\n");
        printf("============================================\n");
        printf("Enter your choice: ");

        int choice;
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                addBook(&library);
                break;
            case 2:
                displayBooks(&library);
                break;
            case 3:
                searchBookByTitle(&library);
                break;
            case 4:
                registerMember(&library);
                break;
            case 5:
                displayMembers(&library);
                break;
            case 6:
                borrowBook(&library);
                break;
            case 7:
                returnBook(&library);
                break;
            case 8:
                displayTransactions(&library);
                break;
            case 9:
                printf("Exiting... Ta..Ta..Bye!\n");
                exit(0);
            default:
                printf("Invalid choice.\n");
        }
    }

    return 0;
}
