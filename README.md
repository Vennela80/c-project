
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct Book {
    char title[50];
    char author[50];
    int publicationYear;
    int isAvailable;
} Book;


typedef struct Library {
    Book books[100];
    int numBooks;
} Library;


void addBook(Library* library) {
    if (library->numBooks < 100) {
        printf("Enter book title: ");
        scanf("%s", library->books[library->numBooks].title);
        printf("Enter book author: ");
        scanf("%s", library->books[library->numBooks].author);
        printf("Enter book publication year: ");
        scanf("%d", &library->books[library->numBooks].publicationYear);
        library->books[library->numBooks].isAvailable = 1;
        library->numBooks++;
        printf("Book added successfully!\n");
    } else {
        printf("Library is full!\n");
    }
}


void displayBooks(Library* library) {
    if (library->numBooks == 0) {
        printf("No books in the library!\n");
    } else {
        printf("Books in the library:\n");
        for (int i = 0; i < library->numBooks; i++) {
            printf("Title: %s\n", library->books[i].title);
            printf("Author: %s\n", library->books[i].author);
            printf("Publication Year: %d\n", library->books[i].publicationYear);
            printf("Availability: %s\n", library->books[i].isAvailable ? "Yes" : "No");
            printf("\n");
        }
    }
}


void borrowBook(Library* library) {
    char title[50];
    printf("Enter book title: ");
    scanf("%s", title);
    for (int i = 0; i < library->numBooks; i++) {
        if (strcmp(library->books[i].title, title) == 0) {
            if (library->books[i].isAvailable) {
                library->books[i].isAvailable = 0;
                printf("Book borrowed successfully!\n");
            } else {
                printf("Book is not available!\n");
            }
            return;
        }
    }
    printf("Book not found!\n");
}
void returnBook(Library* library) {
    char title[50];
    printf("Enter book title: ");
    scanf("%s", title);
    for (int i = 0; i < library->numBooks; i++) {
        if (strcmp(library->books[i].title, title) == 0) {
            library->books[i].isAvailable = 1;
            printf("Book returned successfully!\n");
            return;
        }
    }
    printf("Book not found!\n");
}

int main() {
    Library library;
    library.numBooks = 0;

    while (1) {
        printf("Library Management System\n");
        printf("1. Add Book\n");
        printf("2. Display Books\n");
        printf("3. Borrow Book\n");
        printf("4. Return Book\n");
        printf("5. Exit\n");
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
                borrowBook(&library);
                break;
            case 4:
                returnBook(&library);
                break;
            case 5:
                exit(0);
            default:
                printf("Invalid choice!\n");
        }
    }

    return 0;
}
