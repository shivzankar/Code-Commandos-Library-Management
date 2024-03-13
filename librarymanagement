import tkinter as tk
from tkinter import messagebox, simpledialog
import mysql.connector

# Connect to MySQL database
mydb = mysql.connector.connect(
    host="localhost",
    user="root",
    password="root",
    database="library"
)
mycursor = mydb.cursor()

# Function to add a book to the database
def add_book_window():
    add_window = tk.Toplevel(root)
    add_window.title("Add Book")

    title_label = tk.Label(add_window, text="Title:")
    title_label.grid(row=0, column=0, padx=10, pady=5)
    title_entry = tk.Entry(add_window)
    title_entry.grid(row=0, column=1, padx=10, pady=5)

    author_label = tk.Label(add_window, text="Author:")
    author_label.grid(row=1, column=0, padx=10, pady=5)
    author_entry = tk.Entry(add_window)
    author_entry.grid(row=1, column=1, padx=10, pady=5)

    quantity_label = tk.Label(add_window, text="Quantity:")
    quantity_label.grid(row=2, column=0, padx=10, pady=5)
    quantity_entry = tk.Entry(add_window)
    quantity_entry.grid(row=2, column=1, padx=10, pady=5)

    add_button = tk.Button(add_window, text="Add", command=lambda: add_book(title_entry.get(), author_entry.get(), quantity_entry.get()))
    add_button.grid(row=3, columnspan=2, padx=10, pady=5)

def add_book(title, author, quantity):
    if title and author and quantity:
        mycursor.execute("INSERT INTO books (title, author, quantity) VALUES (%s, %s, %s)", (title, author, quantity))
        mydb.commit()
        messagebox.showinfo("Success", "Book added successfully")
    else:
        messagebox.showerror("Error", "Please fill in all fields.")

# Function to search for a book
def search_book():
    title = simpledialog.askstring("Search Book", "Enter title to search:")
    if title:
        mycursor.execute("SELECT * FROM books WHERE title LIKE %s", ('%' + title + '%',))
        books = mycursor.fetchall()

        if books:
            search_results = "\n".join(f"Title: {book[1]}, Author: {book[2]}, Quantity: {book[3]}" for book in books)
            messagebox.showinfo("Search Results", search_results)
        else:
            messagebox.showinfo("Not Found", "No books found matching the search criteria")

# Function to borrow a book
def borrow_book_window():
    borrow_window = tk.Toplevel(root)
    borrow_window.title("Borrow Book")

    book_id_label = tk.Label(borrow_window, text="Book ID:")
    book_id_label.grid(row=0, column=0, padx=10, pady=5)
    book_id_entry = tk.Entry(borrow_window)
    book_id_entry.grid(row=0, column=1, padx=10, pady=5)

    borrower_name_label = tk.Label(borrow_window, text="Your Name:")
    borrower_name_label.grid(row=1, column=0, padx=10, pady=5)
    borrower_name_entry = tk.Entry(borrow_window)
    borrower_name_entry.grid(row=1, column=1, padx=10, pady=5)

    borrower_email_label = tk.Label(borrow_window, text="Your Email:")
    borrower_email_label.grid(row=2, column=0, padx=10, pady=5)
    borrower_email_entry = tk.Entry(borrow_window)
    borrower_email_entry.grid(row=2, column=1, padx=10, pady=5)

    borrower_phone_label = tk.Label(borrow_window, text="Your Phone:")
    borrower_phone_label.grid(row=3, column=0, padx=10, pady=5)
    borrower_phone_entry = tk.Entry(borrow_window)
    borrower_phone_entry.grid(row=3, column=1, padx=10, pady=5)

    borrow_button = tk.Button(borrow_window, text="Borrow", command=lambda: borrow_book(book_id_entry.get(), borrower_name_entry.get(), borrower_email_entry.get(), borrower_phone_entry.get()))
    borrow_button.grid(row=4, columnspan=2, padx=10, pady=5)

def borrow_book(book_id, name, email, phone):
    if book_id and name and email and phone:
        mycursor.execute("INSERT INTO borrowers (name, email, phone, book_id) VALUES (%s, %s, %s, %s)", (name, email, phone, book_id))
        mydb.commit()
        messagebox.showinfo("Success", "Book borrowed successfully")
    else:
        messagebox.showerror("Error", "Please fill in all fields.")

# Function to delete a book
def delete_book():
    selected_book_index = books_listbox.curselection()
    if selected_book_index:
        selected_book_id = books_listbox.get(selected_book_index)[0]
        mycursor.execute("DELETE FROM books WHERE id = %s", (selected_book_id,))
        mydb.commit()
        messagebox.showinfo("Success", "Book deleted successfully")
        refresh_books_listbox()
    else:
        messagebox.showerror("Error", "Please select a book to delete.")

# Function to show available books
def show_available_books():
    mycursor.execute("SELECT * FROM books WHERE quantity > 0")
    available_books = mycursor.fetchall()

    if available_books:
        available_books_text = "\n".join(f"Title: {book[1]}, Author: {book[2]}, Quantity: {book[3]}" for book in available_books)
        messagebox.showinfo("Available Books", available_books_text)
    else:
        messagebox.showinfo("No Available Books", "There are no available books.")

# Function to refresh the listbox with books
def refresh_books_listbox():
    mycursor.execute("SELECT * FROM books")
    all_books = mycursor.fetchall()
    books_listbox.delete(0, tk.END)
    for book in all_books:
        books_listbox.insert(tk.END, book)

# Create the main window
root = tk.Tk()
root.title("Library Management System")

# Create listbox to display books
books_listbox = tk.Listbox(root, width=50)
books_listbox.pack(pady=10)
refresh_books_listbox()

# Create buttons for actions
add_button = tk.Button(root, text="Add Book", command=add_book_window)
add_button.pack(pady=5)

search_button = tk.Button(root, text="Search Book", command=search_book)
search_button.pack(pady=5)

borrow_button = tk.Button(root, text="Borrow Book", command=borrow_book_window)
borrow_button.pack(pady=5)

delete_button = tk.Button(root, text="Delete Selected Book", command=delete_book)
delete_button.pack(pady=5)

available_button = tk.Button(root, text="Available Books", command=show_available_books)
available_button.pack(pady=5)

root.mainloop()
