# FastAPI Blog

A small blog application built with FastAPI, SQLAlchemy, SQLite, and Jinja2. It serves HTML pages for browsing posts and JSON API routes for users and posts. User accounts currently contain a username and email; authentication and passwords are not implemented.

## Requirements

- Python 3.12 or newer
- [uv](https://docs.astral.sh/uv/)

## Run locally

From the project root, install dependencies and start the development server:

```powershell
uv sync
uv run fastapi dev main.py
```

Open <http://127.0.0.1:8000> for the blog. The interactive API documentation is at <http://127.0.0.1:8000/docs>.

The application uses `sqlite:///./blog.db`, so SQLite creates or opens `blog.db` in the current working directory. Tables are created at startup with SQLAlchemy's `Base.metadata.create_all()`. This creates missing tables; it does not migrate existing tables when model definitions change.

## HTML pages

| Method | Path | Description |
| --- | --- | --- |
| `GET` | `/` | Blog home page with posts |
| `GET` | `/posts` | Same post listing as the home page |
| `GET` | `/post/{post_id}` | Page for one post |
| `GET` | `/users/{user_id}/posts` | Page listing posts by one user |

The app serves assets from `static/` at `/static` and uploaded or user media from `media/` at `/media`. Users without a custom image have a default image path of `/static/profile_pics/default.jpg`.

## JSON API

| Method | Path | Description |
| --- | --- | --- |
| `POST` | `/api/users` | Create a user with a username and email |
| `GET` | `/api/user/{user_id}` | Get one user |
| `GET` | `/api/users/{user_id}/posts` | Get a user's posts |
| `GET` | `/api/posts` | Get all posts |
| `GET` | `/api/post/{post_id}` | Get one post |
| `POST` | `/api/post` | Create a post for an existing user |

Example user request:

```json
{
  "username": "Shiran",
  "email": "shiran@example.com"
}
```

Example post request:

```json
{
  "title": "My first post",
  "content": "Hello from the blog.",
  "user_id": 1
}
```

API requests with invalid input receive a `422` JSON response. Missing users or posts receive `404`; duplicate usernames or email addresses receive `400` when creating a user.

## Data model

- A user has a unique username and email, and may have an optional profile image filename.
- A post has a title, content, posting date, and a required author (`user_id`).
- Posts are stored in SQLite and remain across application restarts.

## Project layout

```text
fastapi_blog/
├── main.py           # FastAPI app, HTML routes, and JSON API
├── models.py         # SQLAlchemy User and Post models
├── schemas.py        # Pydantic request and response schemas
├── database.py       # SQLite engine and session dependency
├── templates/        # Jinja2 HTML templates
├── static/           # CSS, icons, and default profile image
├── media/            # User media files
├── pyproject.toml    # Project metadata and dependencies
└── uv.lock           # Locked dependency versions
```

Run the server from the project root because the database, template, static, and media paths are relative to the current working directory.
