# hello-py
import json
import os

FILE = "library.json"

# -------------------- Data --------------------

def load_books():
    if os.path.exists(FILE):
        with open(FILE, "r") as f:
            return json.load(f)
    return []

def save_books(books):
    with open(FILE, "w") as f:
        json.dump(books, f, indent=4)

# -------------------- Functions --------------------

def add_book(books):
    print("\n--- Add Book ---")
    book = {
        "id": input("Book ID: "),
        "title": input("Title: "),
        "author": input("Author: "),
        "year": input("Year: "),
        "available": True
    }
    books.append(book)
    save_books(books)
    print("Book added successfully!")

def view_books(books):
    print("\n--- Library Books ---")
    if not books:
        print("No books found.")
        return

    for b in books:
        status = "Available" if b["available"] else "Issued"
        print(f"""
ID      : {b['id']}
Title   : {b['title']}
Author  : {b['author']}
Year    : {b['year']}
Status  : {status}
---------------------------""")

def search_book(books):
    keyword = input("Enter title or author: ").lower()
    found = False

    for b in books:
        if keyword in b["title"].lower() or keyword in b["author"].lower():
            print(f"{b['id']} | {b['title']} | {b['author']}")
            found = True

    if not found:
        print("Book not found.")

def issue_book(books):
    book_id = input("Enter Book ID to issue: ")

    for b in books:
        if b["id"] == book_id:
            if b["available"]:
                b["available"] = False
                save_books(books)
                print("Book issued successfully!")
            else:
                print("Book is already issued.")
            return

    print("Book ID not found.")

def return_book(books):
    book_id = input("Enter Book ID to return: ")

    for b in books:
        if b["id"] == book_id:
            if not b["available"]:
                b["available"] = True
                save_books(books)
                print("Book returned successfully!")
            else:
                print("This book is already available.")
            return

    print("Book ID not found.")

def delete_book(books):
    book_id = input("Enter Book ID to delete: ")

    for b in books:
        if b["id"] == book_id:
            books.remove(b)
            save_books(books)
            print("Book deleted.")
            return

    print("Book not found.")

def statistics(books):
    total = len(books)
    available = sum(1 for b in books if b["available"])
    issued = total - available

    print("\n--- Library Statistics ---")
    print("Total Books     :", total)
    print("Available Books :", available)
    print("Issued Books    :", issued)

# -------------------- Main Menu --------------------

def main():
    books = load_books()

    while True:
        print("""
========= LIBRARY MANAGEMENT =========
1. Add Book
2. View Books
3. Search Book
4. Issue Book
5. Return Book
6. Delete Book
7. Statistics
8. Exit
====================================
""")

        choice = input("Enter choice: ")

        if choice == "1":
            add_book(books)
        elif choice == "2":
            view_books(books)
        elif choice == "3":
            search_book(books)
        elif choice == "4":
            issue_book(books)
        elif choice == "5":
            return_book(books)
        elif choice == "6":
            delete_book(books)
        elif choice == "7":
            statistics(books)
        elif choice == "8":
            print("Thank you for using the Library System!")
            break
        else:
            print("Invalid choice. Try again.")

if __name__ == "__main__":
    main()
