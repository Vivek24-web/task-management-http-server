# Task Management HTTP Server

A small task API built with Node.js and TypeScript. It uses Node's built-in `http` module, so there is no Express or database. Tasks are stored in `data/tasks.json` and remain there after the server stops.

## Requirements

- Node.js (v20 or newer recommended)
- npm

## Setup

Open a terminal in this project folder and run:

```bash
npm install
npm run build
npm start
```

The server starts at `http://localhost:3000`. To use another port, set the `PORT` environment variable before starting it.

**Windows PowerShell:** if `npm` is blocked by your PowerShell execution policy, use `npm.cmd` instead:

```powershell
npm.cmd install
npm.cmd run build
npm.cmd start
```

## Endpoints

| Method | Path | Purpose |
| --- | --- | --- |
| GET | `/health` | Check that the server is running |
| POST | `/tasks` | Create a task |
| GET | `/tasks` | Get all tasks |
| GET | `/tasks/:id` | Get one task |
| PATCH | `/tasks/:id` | Update some task fields |
| DELETE | `/tasks/:id` | Delete a task |

Valid statuses: `todo`, `in-progress`, `done`  
Valid priorities: `low`, `medium`, `high`

New tasks default to status `todo`, priority `medium`, and an empty description. IDs are generated automatically. Creation and update times are stored as ISO timestamps.

## Example requests

### Health check

```bash
curl http://localhost:3000/health
```

### Create a task

```bash
curl -X POST http://localhost:3000/tasks -H "Content-Type: application/json" -d '{"title":"Finish assignment","description":"Test every endpoint","priority":"high"}'
```

Copy the `id` from the response and use it in the examples below.

### Get all tasks

```bash
curl http://localhost:3000/tasks
```

### Filter tasks

```bash
curl "http://localhost:3000/tasks?status=todo"
curl "http://localhost:3000/tasks?priority=high"
curl "http://localhost:3000/tasks?status=todo&priority=high"
```

### Get one task

```bash
curl http://localhost:3000/tasks/YOUR_TASK_ID
```

### Update a task

```bash
curl -X PATCH http://localhost:3000/tasks/YOUR_TASK_ID -H "Content-Type: application/json" -d '{"status":"in-progress","priority":"medium"}'
```

### Delete a task

```bash
curl -X DELETE http://localhost:3000/tasks/YOUR_TASK_ID
```

On Windows, you can also use Postman or the VS Code Thunder Client extension to send these requests. In PowerShell, `curl` may be an alias; `curl.exe` avoids that issue.

## Error responses

- `400 Bad Request` — malformed JSON, missing title, invalid status/priority, or invalid filter
- `404 Not Found` — unknown route or task ID
- `413 Payload Too Large` — request body exceeds the size limit
- `500 Internal Server Error` — unexpected server-side problem

## Project structure

```text
src/
  server.ts      # HTTP routes and request validation
  taskStore.ts   # Reading/writing tasks and CRUD functions
  types.ts       # Task and input types
```

The `data/tasks.json` file is created automatically the first time the API reads or writes tasks. Do not delete it if you want to keep the saved tasks.
