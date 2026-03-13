---
name: bun-cli-creation
description: Creates CLI applications using Bun runtime with SQLite, TypeScript support, and yargs for command parsing. Use when you need to build command-line tools with database persistence and modern JavaScript features.
license: Apache-2.0
metadata:
  version: 0.0.0
  author: trebaud
---

# Bun CLI Creation

Creates high-performance command-line applications using Bun runtime with built-in SQLite, TypeScript, and package management.

## When to Use
- Building CLI tools that need database persistence
- Requiring fast development with built-in TypeScript support
- Creating command-line apps with argument parsing and help generation
- Projects needing SQLite database integration without external dependencies

## Core Setup

### Project Initialization
```bash
mkdir project-name && cd project-name
bun init -y
```

### CLI Registration
Add to `package.json`:
```json
"bin": {
    "cli-name": "./index.ts"
}
```

Add shebang to `index.ts`:
```typescript
#! /usr/bin/env bun
```

Link globally:
```bash
bun link
bun link <project_name>
```

## Database Layer

### SQLite Setup (`src/db.ts`)
```typescript
import { Database } from "bun:sqlite"

const db = new Database("../db.sqlite")
db.exec("PRAGMA journal_mode = WAL;")

export const note_table_query = db.prepare(`CREATE TABLE IF NOT EXISTS note (
  note_id INTEGER PRIMARY KEY AUTOINCREMENT,
  note TEXT NOT NULL
)`)

export function create_new_note(note: string): void {
  const query = db.query(`INSERT INTO note (note) VALUES (?)`);
  query.run(note);
}
```

## Command Layer

### Yargs Configuration (`src/command.ts`)
```typescript
import yargs from "yargs"
import { hideBin } from "yargs/helpers"
import { create_note_handler } from "./handler"

yargs(hideBin(process.argv))
  .command(
    "new <note>",
    "Creates a new Note",
    (yargs) => yargs.positional("note", {
      description: "The content of the note",
      type: "string",
    }),
    (argv) => create_note_handler(argv.note as string),
  )
  .parse()
```

## Service Layer

### Business Logic (`src/handler.ts`)
```typescript
import { create_new_note } from "./db"

export function create_note_handler(note: string) {
  try {
    create_new_note(note);
    console.log(`${note} added successfully`);
  } catch (err) {
    console.error(err);
    console.error("Oops, Error!");
  }
}
```

## Main Entry Point

### Bootstrap (`index.ts`)
```typescript
#! /usr/bin/env bun
import "./src/command.ts";
import { note_table_query } from './src/db';

note_table_query.run();
```

## Dependencies

Install required packages:
```bash
bun add yargs
bun add -D @types/yargs
```

## Key Bun Features

- **TypeScript Support**: No configuration needed
- **Built-in SQLite**: High-performance database included
- **Module System**: Supports both ES Modules and CommonJS
- **Package Management**: 30x faster than npm
- **No Build Step**: TypeScript runs directly