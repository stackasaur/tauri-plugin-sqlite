# Tauri Plugin SQLite

[![license](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

This plugin provides a "classical" Tauri Plugin interface to SQLite database through [sqlite](https://github.com/stainless-steel/sqlite).

## Installation

### Rust

`src-tauri/Cargo.toml`

```toml
[dependencies.tauri-plugin-sqlite]
git = "https://github.com/lzdyes/tauri-plugin-sqlite"
tag = "v0.1.0"
# branch = "main"
```

### Web

```
npm install github:lzdyes/tauri-plugin-sqlite#v0.1.0
# or
yarn add github:lzdyes/tauri-plugin-sqlite#v0.1.0
```

## Usage

### Rust

Use in `src-tauri/src/main.rs`:

```rust
fn main() {
    tauri::Builder::default()
        .plugin(tauri_plugin_sqlite::init())
        .build()
        .run();
}
```

### JavaScript/TypeScript

```ts
import SQLite from 'tauri-plugin-sqlite-api'

/** The path will be 'src-tauri/test.db', you can customize the path */
const db = await SQLite.open('./test.db')

/** execute SQL */
await db.execute(`
    CREATE TABLE users (name TEXT, age INTEGER);
    INSERT INTO users VALUES ('Alice', 42);
    INSERT INTO users VALUES ('Bob', 69);
`)

/** execute SQL with params */
await db.execute('INSERT INTO users VALUES (?1, ?2)', ['Jack', 18])

/** batch execution SQL with params */
await db.execute('INSERT INTO users VALUES (?1, ?2)', [
  ['Allen', 20],
  ['Barry', 16],
  ['Cara', 28],
])

/** select count */
const rows = await db.select<Array<{ count: number }>>('SELECT COUNT(*) as count FROM users')

/** select with params */
const rows = await db.select<Array<{ name: string }>>('SELECT name FROM users WHERE age > ?', [20])

const rows = await db.select<Array<any>>('SELECT * FROM users LIMIT ?1 OFFSET ?2', [10, 0])
```

## License

[MIT](LICENSE)
