# App

Sample Flask REST API for managing comments.

## Endpoints

| Method | Path | Description |
|---|---|---|
| POST | `/api/comment/new` | Create a new comment |
| GET | `/api/comment/list/<content_id>` | List comments by content ID |

## Run

```bash
pip install -r requirements.txt
python api.py
```
