# Task Tracker

Simple CLI task tracker that stores tasks in JSON files; see the project on [roadmap.sh](https://roadmap.sh/projects/task-tracker).

## Features
- Add tasks with automatic ID generation.
- List tasks (all / todo / done / in-progress).
- Mark tasks with a new status.
- Update task descriptions.
- Delete tasks.
- Persistent storage in [data.json](data.json) and last generated ID in [last_id.json](last_id.json).

## Quick start

Run locally with Python:

```sh
python main.py <command> [args]
```

Example commands:

- Add a task
```sh
python main.py add "Buy groceries"
```

- List all tasks
```sh
python main.py list
```

- List todo tasks
```sh
python main.py list --todo
```

- List done tasks
```sh
python main.py list --done
```

- List in-progress tasks
```sh
python main.py list --in-progress
```

- Mark a task status (accepted statuses: `todo`, `done`, `in-progres`)
```sh
python main.py mark 0 done
python main.py mark 1 in-progres
```

- Update a task description
```sh
python main.py update 0 "New description"
```

- Delete a task
```sh
python main.py delete 0
```

Note: The CLI entrypoint is implemented in [`main.py`](main.py) as the [`main.main`](main.py) function. Argument parsing and command handlers are defined in [`sysargs.py`](sysargs.py) via [`sysargs.create_parser`](sysargs.py).

## Implementation overview

Core logic lives in [`task_property.py`](task_property.py):

- Class [`task_property.Task`](task_property.py) — in-memory Task object.
- Class [`task_property.GeneradorID`](task_property.py) and instance [`task_property.generador_id`](task_property.py) — singleton ID generator that initializes from existing data.
- Functions:
  - [`task_property.add`](task_property.py) — create and persist a new task.
  - [`task_property.update`](task_property.py) — update task description.
  - [`task_property.mark`](task_property.py) — change task status.
  - [`task_property.list`](task_property.py) — list tasks (optionally filtered).
  - [`task_property.delete`](task_property.py) — remove a task.
  - [`task_property.save`](task_property.py) — append a task to the JSON store.
  - [`task_property.obj_to_json`](task_property.py) — serialize Task to JSON.

Files:
- [main.py](main.py) — app entrypoint.
- [sysargs.py](sysargs.py) — CLI argument parsing and handlers.
- [task_property.py](task_property.py) — task model and persistence.
- [pyproject.toml](pyproject.toml) — project metadata and script entry (task-tracker).
- [data.json](data.json) — task storage (created at runtime).
- [last_id.json](last_id.json) — last generated ID (created at runtime).

## Notes & Suggestions
- The `mark` command currently accepts the status string `in-progres` (single "s" missing) as defined in the code. See [`sysargs.py`](sysargs.py) and [`task_property.py`](task_property.py) for the exact accepted values.
- The project script entry is defined in [pyproject.toml](pyproject.toml) as `task-tracker = "task-tracker.main:main"`. You can install the package to expose the `task-tracker` console script.

## License
Add your license here.