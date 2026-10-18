# FastAPI Blog

A small blog application built with FastAPI and Jinja2. It serves HTML pages for the blog and JSON endpoints for posts. Posts are currently stored in memory, so changes reset when the application restarts.

## Requirements

- Python 3.12 or newer
- [uv](https://docs.astral.sh/uv/)

## Run locally

From the project root, install the dependencies and start the development server:

```powershell
uv sync
uv run fastapi dev main.py
```

Open <http://127.0.0.1:8000> in a browser. FastAPI's interactive API documentation is available at <http://127.0.0.1:8000/docs>.

## Routes

| Method | Path | Description |
| --- | --- | --- |
| `GET` | `/` | Blog home page |
| `GET` | `/posts` | Blog home page |
| `GET` | `/post/{post_id}` | Page for one post |
| `GET` | `/api/posts` | Return all posts as JSON |
| `GET` | `/api/post/{post_id}` | Return one post as JSON |

Unknown post IDs return a 404 response. Browser page errors use the `templates/error.html` template; API errors return JSON.

## Project layout

```text
fastapi_blog/
├── main.py           # FastAPI application and routes
├── templates/        # Jinja2 HTML templates
├── static/           # CSS, images, and other static assets
├── pyproject.toml    # Project metadata and dependencies
└── uv.lock           # Locked dependency versions
```

The app expects to be started from the project root because the template and static directories are configured with relative paths.
