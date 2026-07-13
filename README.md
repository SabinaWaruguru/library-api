📚 Library API - C027-01-2709/2024
A simple FastAPI application for managing books and categories in a library system. Built with SQLModel for database interactions, backed by PostgreSQL.

🚀 Features
Create categories
Add books with metadata
List books with filters (pagination, availability)
Search books by author or title
Retrieve book details by ID
Update book records (partial updates supported)
🛠️ Tech Stack
Tool	Purpose
FastAPI	Web framework
SQLModel	ORM + Pydantic validation combined
PostgreSQL	Database (via Docker)
psycopg2-binary	PostgreSQL driver
python-dotenv	Loads config from .env
Docker Compose	Runs PostgreSQL locally
⚙️ Installation
1. Clone the repository

git clone https://github.com/your-username/library-api.git
cd library-api
2. Create a virtual environment

python -m venv venv
source venv/bin/activate   # Linux/Mac
venv\Scripts\activate      # Windows
3. Install dependencies

pip install -r requirements.txt
4. Configure environment variables

Create a .env file in the project root:

DATABASE_URL=postgresql://postgres:1234567890@localhost:5433/library_db
5. Start PostgreSQL

docker compose up -d
Verify it's running:

docker ps
6. Run the application

uvicorn main:app --reload
The API will be available at http://localhost:8000, with interactive docs at http://localhost:8000/docs.

🗂️ Project Structure
library-api/
│── main.py                # FastAPI app & routes
│── database/
│   ├── __init__.py
│   └── session.py         # Database session & engine
│── models/
│   ├── __init__.py
│   ├── book.py             # Book models (Book, BookCreate, BookUpdate)
│   └── category.py         # Category model
│── .env                    # DATABASE_URL (not committed to git)
│── docker-compose.yml      # PostgreSQL container config
│── requirements.txt        # Dependencies
│── README.md               # Documentation
📖 API Endpoints
Root

GET / → Welcome message
Categories

POST /categories → Create a new category
Books

POST /books → Create a new book
GET /books → List books (filters: skip, limit, available)
GET /books/search → Search books (author, title)
GET /books/{book_id} → Get book by ID
PATCH /books/{book_id} → Update book details
🛠️ Example Requests
Create a Category

curl -X POST "http://127.0.0.1:8000/categories?name=Mystery"
Add a Book

curl -X POST "http://127.0.0.1:8000/books" \
-H "Content-Type: application/json" \
-d '{"title":"Kill","author":"Cathy","isbn":"9780451524935","published_year":2017,"category_id":1}'
Search Books

curl "http://127.0.0.1:8000/books/search?author=Cathy"
Update a Book

curl -X PATCH "http://127.0.0.1:8000/books/1" \
-H "Content-Type: application/json" \
-d '{"available":false}'
📋 Data Models
Book

id, title, author, isbn (unique), published_year, available, created_at, updated_at, category_id
Category

id, name (unique), related books
📝 Notes
Route ordering matters: /books/search is declared before /books/{book_id} so FastAPI doesn't try to interpret "search" as a book ID.
BookUpdate uses exclude_unset=True so PATCH requests only modify the fields explicitly sent, not all fields.
To reset the database schema after model changes: docker compose down -v && docker compose up -d.
👤 Author
Sabina Wairimu 