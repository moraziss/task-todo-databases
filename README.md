# task-todo-databases

PostgreSQL schema for the Task Todo app ([frontend](https://github.com/moraziss/task-todo-frontend), [backend](https://github.com/moraziss/task-todo-backend)).

## Contents

| File | Description |
|------|-------------|
| `data_bases/queries/Query_4.sql` | Drops and recreates the schema, adds indexes and the default category |

## Schema

- **categories**: `id` (UUID), `name` (unique), `color`, `is_default`, timestamps.
- **tasks**: `id`, optional `parent_id` (self-reference for subtasks, cascade delete), `title`, `priority` (default `medium`), `category_id` (set to NULL when the category is deleted), `is_completed`, `is_pinned`, `is_deleted` (soft delete), `deadline`, timestamps.
- **attachments**: `id`, `task_id` (cascade delete), `file_name`, `file_path`, `file_size`, `uploaded_at`.

Indexes: `tasks(parent_id)`, `tasks(is_deleted)`, `tasks(is_pinned)`, `tasks(deadline)`.

A default category `Общие` with the nil UUID `00000000-0000-0000-0000-000000000000` is inserted; the client expects this exact ID.

## Usage

```bash
psql -d <database> -f data_bases/queries/Query_4.sql
```

> **Warning:** the script starts with `DROP TABLE IF EXISTS` for all three tables, so every existing row is deleted. Do not run it against a database with data you want to keep.
