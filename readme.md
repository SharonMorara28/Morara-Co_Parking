# Morara&Co Parking Management System 🚗

Hey! I wanted to build something practical that solves an actual problem in Rongai Area in Kajiado.

It's a complete, single-file parking management web app powered by SQLite running right inside your browser!

---

## What does it do?

The app lets parking attendants track and manage parking spot availability in real time:

* **Visual Grid Layout**: Displays 10 parking spots with clear visual status (Green = Free, Red = Occupied).
* **Live Spot Check-In**: Assign an incoming vehicle's license plate to any open spot with an `INSERT` statement.
* **Auto Fee Calculation**: Select an occupied spot to check out a vehicle. The app calculates hours parked and charges **$2.00/hr** (minimum 1 hour).
* **SQLite Inspector**: Switch between viewing raw `spots` and `vehicles` tables to see real-time updates to the database.
* **Live Terminal Console**: Shows every SQL query (`CREATE`, `INSERT`, `UPDATE`, `LEFT JOIN`) executed under the hood as you interact with the app.


##  How it works

This is a **self-contained web application**, I made sure everything runs inside a single `index.html` file so you don't have to install NodeJS, Python, or set up local SQL servers.

* **HTML5 & Tailwind CSS**: Used Tailwind via CDN so I didn't have to write hundreds of lines of raw CSS styles.
* **JavaScript (Vanilla ES6)**: Handles DOM manipulation, fee math, timestamp formatting, and events.
* **sql.js (WebAssembly SQLite)**: Uses standard SQLite3 compiled to WebAssembly. It creates and queries an actual SQL database directly inside browser memory (`RAM`).

##  How to Run It

Because it's completely self-contained, running it is super easy:

1. Download or clone this repository.
2. Open `index.html` directly in any web browser (Chrome, Firefox, Edge, Safari).
3. That's literally it!

## Database Scheme

The app creates two relational SQLite tables on load:

```sql
-- Table 1: Stores spot status
CREATE TABLE spots (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    spot_number INTEGER NOT NULL UNIQUE,
    is_occupied INTEGER DEFAULT 0
);

-- Table 2: Tracks vehicle sessions and calculated fees
CREATE TABLE vehicles (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    spot_id INTEGER NOT NULL,
    license_plate TEXT NOT NULL,
    entry_time TEXT NOT NULL,
    exit_time TEXT,
    total_fee REAL DEFAULT 0.0,
    FOREIGN KEY(spot_id) REFERENCES spots(id)
);
```
## Algorithms used
1.B-Tree Data Indexing and SearchAlgorithm: 
B-Tree (and B+Tree) Search and Insertion. Application: SQLite stores primary keys, index structures, and row records inside balanced B-Tree structures.Time Complexity is $O(\log N)$ search time for primary key lookups (WHERE id = ? or WHERE slot_number = ?) compared to linear $O(N)$ full table scans.

2.Relational Hash Algorithm: 
Nested Loop Join / Hash Join execution.Application: Executed when querying occupied slots using relational queries:

