# 📄 colorscheme-persist

Like `:Telescope colorscheme` but saving the selection to a file so the
colorscheme can persist the next time you open neovim. Without having to
manually modify your config files.


# 📏 Requirements

- Neovim >= 0.5
- [telescope.nvim](https://github.com/nvim-telescope/telescope.nvim)


# 📦 Installation

### [packer](https://github.com/wbthomason/packer.nvim)

```lua
use({
  "propet/colorscheme-persist.nvim",
  requires = {
    "nvim-telescope/telescope-dap.nvim"
  }
})
```


# 🚀 Usage

```lua
local persist_colorscheme = require("colorscheme-persist")

-- Setup
persist_colorscheme.setup()

-- Get stored colorscheme
local colorscheme = persist_colorscheme.get_colorscheme()

-- Set colorscheme
vim.cmd("colorscheme " .. colorscheme)

-- Keymap for telescope selection
vim.api.nvim_set_keymap(
  "n",
  "<leader>sc",
  ":lua require('colorscheme-persist').picker()<CR>",
  { noremap = true, silent = true, desc = "colorscheme-persist" }
)
```


# 📡 Configuration

There are some configuration options provided with the following default
values:

```lua
require("colorscheme-persist").setup({
  -- Absolute path to file where colorscheme should be saved
  file_path = os.getenv("HOME") .. "/.nvim.colorscheme-persist.lua",
  -- In case there's no saved colorscheme yet
  fallback = "default",
  -- List of ugly colorschemes to avoid in the selection window
  disable = {
    "darkblue",
    "default",
    "delek",
    "desert",
    "elflord",
    "evening",
    "industry",
    "koehler",
    "morning",
    "murphy",
    "pablo",
    "peachpuff",
    "ron",
    "shine",
    "slate",
    "torte",
    "zellner"
  },
  -- Options for the telescope picker
  picker_opts = require("telescope.themes").get_dropdown()
})
```
