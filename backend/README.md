# Backend - Trivia API

## Setting up the Backend

### Install Dependencies

1. **Python 3.7** - Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

2. **Virtual Environment** - We recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organized. Instructions for setting up a virual environment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)

3. **PIP Dependencies** - Once your virtual environment is setup and running, install the required dependencies by navigating to the `/backend` directory and running:

```bash
pip install -r requirements.txt
```

#### Key Pip Dependencies

- [Flask](http://flask.pocoo.org/) is a lightweight backend microservices framework. Flask is required to handle requests and responses.

- [SQLAlchemy](https://www.sqlalchemy.org/) is the Python SQL toolkit and ORM we'll use to handle the lightweight SQL database. You'll primarily work in `app.py`and can reference `models.py`.

- [Flask-CORS](https://flask-cors.readthedocs.io/en/latest/#) is the extension we'll use to handle cross-origin requests from our frontend server.

### Set up the Database

With Postgres running, create a `trivia` database:

```bash
createbd trivia
```

Populate the database using the `trivia.psql` file provided. From the `backend` folder in terminal run:

```bash
psql trivia < trivia.psql
```

### Run the Server

From within the `./src` directory first ensure you are working using your created virtual environment.

To run the server, execute:

```bash
flask run --reload
```

The `--reload` flag will detect file changes and restart the server automatically.

## API Documentation

### Endpoints

`GET '/categories'`

- Fetches a dictionary of categories in which the keys are the ids and the value is the corresponding string of the category
- Request Arguments: None
- Returns: An object with a single key, `categories`, that contains an object of `id: category_string` key: value pairs.

```json
{
  "1": "Science",
  "2": "Art",
  "3": "Geography",
  "4": "History",
  "5": "Entertainment",
  "6": "Sports"
}
```

-Error example:

```json
{
  "success": false,
  "error": 404,
  "message": "resource not found"
}
```

---

`GET '/questions'`

- Fetches a paginated set of questions, total number of questions integer, all categories and current category string
- Request Arguments: 'page' - integer
- Returns an object with 10 questions, total number of questions, all categories and current category which is null if not currently set

```json
{
  "questions": [
    {
      "id": 1,
      "question": "Whose autobiography is entitled 'I Know Why the Caged Bird Sings'?",
      "answer": "Maya Angelou",
      "difficulty": 2,
      "category": 5
    }
  ],
  "total_questions": 20,
  "categories": {
    "1": "Science",
    "2": "Art",
    "3": "Geography",
    "4": "History",
    "5": "Entertainment",
    "6": "Sports"
  },
  "current_category": "Science"
}
```

-Error example:

```json
{
  "success": false,
  "error": 404,
  "message": "resource not found"
}
```

---

`GET '/categories/id/questions'`

- Fetch questions for a category specified by id request argument
- Request Arguments: `id` - integer
- Returns: An object with questions for the specified category, total number of questions interger, and current category string

```json
{
  "questions": [
    {
      "id": 1,
      "question": "Whose autobiography is entitled 'I Know Why the Caged Bird Sings'?",
      "answer": "Maya Angelou",
      "difficulty": 2,
      "category": 5
    }
  ],
  "total_questions": 20,
  "current_category": "Entertainment"
}
```

-Error example:

```json
{
  "success": false,
  "error": 404,
  "message": "resource not found"
}
```

---

`POST '/questions'`

- Sends a post request to add a new question to the database
- Request Body:

```json
{
  "question": "Who let the dogs out?",
  "answer": "who who who who",
  "difficulty": 5,
  "category": 5
}
```

- Returns: Does not return any new data
  -Error example:

```json
{
  "success": false,
  "error": 422,
  "message": "unprocessable"
}
```

---

`POST '/question/search'`

- Sends a post request to search for a specific question by search term string
- Request Body:

```json
{
  "searchTerm": "Who let the"
}
```

- Returns: the current category string, total number of questions interger, a array of questions whos text match the search term

```json
{
  "questions": [
    {
      "id": 21,
      "question": "Who let the dogs out?",
      "answer": "who who who who",
      "difficulty": 5,
      "category": 5
    }
  ],
  "total_questions": 10,
  "current_category": "Entertainment"
}
```

-Error example:

```json
{
  "success": false,
  "error": 422,
  "message": "unprocessable"
}
```

---

`POST '/quizzes'`

- Sends a post request in order to get the next question
- Request Body:

```json
{
  "previous_questions": [4, 2, 9],
  "quiz_category": {
    "id": 1,
    "type": "science"
  }
}
```

- Returns: A new question object and a array of previous question ids

```json
{
  "question": {
    "id": 1,
    "question": "This is a question",
    "answer": "This is an answer",
    "difficulty": 5,
    "category": 4
  },
  "previousQuestions": [2, 3, 9]
}
```

-Error example:

```json
{
  "success": false,
  "error": 404,
  "message": "resource not found"
}
```

---

`DELETE '/questions/id'`

- Deletes a question using the specified id
- Request Arguments: `id` - integer
- Returns: Does not return any data.

-Error example:

```json
{
  "success": false,
  "error": 404,
  "message": "resource not found"
}
```

---

## Testing

Write at least one test for the success and at least one error behavior of each endpoint using the unittest library.

To deploy the tests, run

```bash
dropdb trivia_test
createdb trivia_test
psql trivia_test < trivia.psql
python test_flaskr.py
```
