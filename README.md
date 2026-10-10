# Object Oriented Programming - Lab

A library management system built with Python classes and inheritance. This lab is part of the Flatiron School Data Science course (Course 0, Week 1, Module 7).

## Overview

Libraries hold different kinds of items (books, magazines, DVDs) that share some behaviour but also have details of their own. This project models that with a base class, `LibraryItem`, and three child classes that inherit from it and extend it.

## Learning Objectives

- Define a parent (base) class
- Define specific attributes and methods
- Define child classes using inheritance
- Instantiate objects and access or run their attributes and methods

## Class Hierarchy

```
LibraryItem
├── Book
├── Magazine
└── DVD
```

### `LibraryItem` (base class)

| Attribute | Type | Notes |
|---|---|---|
| `title` | str | Title of the item |
| `author` | str | Author of the item |
| `publication_year` | int | Year published |
| `is_available` | bool | Defaults to `True` |

| Method | Description |
|---|---|
| `display_info()` | Prints the item's attributes using f-strings |
| `checkout()` | Sets `is_available` to `False` |
| `return_item()` | Sets `is_available` to `True` |

### `Book(LibraryItem)`

- Adds `isbn` (int) and `publisher` (str)
- Calls `super().__init__()` for the shared attributes
- Overrides `display_info()` and uses `super()` to print the shared fields before the book-specific ones

### `Magazine(LibraryItem)`

- Adds `issue_number` (int) and `publication_month` (str)
- Calls `super().__init__()` for the shared attributes
- Overrides `display_info()` to include the magazine-specific fields

### `DVD(LibraryItem)`

- Adds `duration` (minutes, int), `director` (str) and `genres` (set)
- `author` defaults to `'N/A'`
- New method `add_genre(genre)` adds a genre string to the `genres` set

## Example Usage

```python
book1 = Book(title='Dracula', author='Bram Stoker', publication_year=1897,
             isbn=1000, publisher='Archibald')

dvd1 = DVD(title='The Matrix', publication_year=1999, duration=136,
           director='The Wachowskis', genres={'action', 'sci-fi'})

book1.display_info()
# Title: Dracula
# Author: Bram Stoker
# Publication Year: 1897
# Available: True
# ISBN: 1000
# Publisher: Archibald

book1.checkout()
print(book1.is_available)   # False
book1.return_item()
print(book1.is_available)   # True

dvd1.add_genre('thriller')
print(dvd1.genres)          # {'action', 'sci-fi', 'thriller'} (set order may vary)
```

## Running the Notebook

1. Clone the repository:
```bash
   git clone https://github.com/Wakali-Ian/DS_Course0_Week1_Module7_OOPLab.git
   cd DS_Course0_Week1_Module7_OOPLab
```
2. Activate the course environment:
```bash
   conda activate learn-env
```
3. Start Jupyter and open `oop_lab.ipynb`:
```bash
   jupyter notebook
```
4. Use **Kernel → Restart & Run All** to run every cell.

## Files

| File | Description |
|---|---|
| `oop_lab.ipynb` | Completed lab notebook (Steps 1 to 7) |
| `README.md` | Project overview |

## Possible Improvements (Step 7)

These ideas were not required for the lab:

- Make `checkout()` check whether the item is already checked out
- Add data validation and string standardisation for attributes
- Store items in a database so they can be searched
- Handle multiple copies of the same item
- Add more item types such as `Audiobook` or `Journal`, plus search and sort methods

## Author

Wakali-Ian
