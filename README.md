# sql-strings.nvim

Tree-sitter injections for SQL embedded in code strings marked with `--sql`.

<img width="171" height="347" alt="Comparison between two strings, one with syntax highlighting and one without" src="https://github.com/user-attachments/assets/9a66e699-6de2-433a-9db6-e81434aa4800" />


## Install

Lazy:
```lua
{
  "starflower-sh/sql-strings.nvim",
  dependencies = { "nvim-treesitter/nvim-treesitter" },
}
```

Install the SQL parser:

```vim
:TSInstall sql
```

## Usage
Include `--sql` as a SQL comment to treat your strings as SQL.

Currently supported languages:

Python:
```python
query = """
--sql
SELECT 1;
"""
```

Rust:
```rust
let query = r#"
--sql
SELECT 2;
"#;
```

Javascript & Typescript
```js
const query = `
--sql
SELECT 3;
`
```

## Notes
Using Rust? You may run into an issue with Rust analyser overwriting the syntax highlighting.\
You can fix this by disabling semantic tokens, I would suggest just toggling them off while working with SQL, but having them default back on otherwise.\
You may find the following code example of this helpful.

```lua
local semantic_tokens_enabled = true

vim.api.nvim_create_user_command("ToggleSemanticTokens", function()
  local client = vim.lsp.get_clients({ bufnr = 0, name = "rust_analyzer" })[1]

  if not client then
    return
  end

  semantic_tokens_enabled = not semantic_tokens_enabled

  if semantic_tokens_enabled then
    vim.lsp.semantic_tokens.start(0, client.id)
  else
    vim.lsp.semantic_tokens.stop(0, client.id)
  end
end, {})

```
