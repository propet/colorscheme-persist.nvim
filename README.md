# 📄 colorscheme-persist

Like `:Telescope colorscheme` but saving the selection to a file so the
colorscheme can persist the next time you open neovim. Without having to
manually modify your config files.

![demo](demo.gif)

# ✨ Features

*   Provides a Telescope picker to select installed colorschemes.
*   Saves the selected colorscheme to a file.
*   Automatically loads the saved colorscheme on Neovim startup.
*   Allows excluding specific (e.g., default/undesired) colorschemes from the picker.
*   Configurable file path, fallback colorscheme, and Telescope picker options.

# 📋 Requirements

*   Neovim (latest stable or nightly recommended)
*   [nvim-telescope/telescope.nvim](https://github.com/nvim-telescope/telescope.nvim)
*   Installed colorscheme plugins (e.g., `folke/tokyonight.nvim`, etc.)

# 📦 Installation

Use your favorite package manager.

### [lazy.nvim](https://github.com/folke/lazy.nvim)

```lua
{
  "propet/colorscheme-persist.nvim",
  lazy = false, -- Required: Load on startup to set the colorscheme
  config = true, -- Required: call setup() function
  dependencies = {
    "nvim-telescope/telescope.nvim",
    -- NOTE: Add your colorscheme plugins here if you want lazy.nvim
    -- to manage them directly within this plugin specification.
    -- Otherwise, ensure they are installed elsewhere in your config.
    -- Example:
    -- "folke/tokyonight.nvim",
    -- "rebelot/kanagawa.nvim",
    -- "Th3Whit3Wolf/space-nvim",
  },
  -- Set a keymap to open the picker
  keys = {
    {
      "<leader>sc", -- Or your preferred keymap
      function()
        require("colorscheme-persist").picker()
      end,
      mode = "n",
      desc = "Choose colorscheme",
    },
  },
  -- Optional: Configure the plugin (see Configuration section below)
  opts = {
    -- Add custom options here, for example:
    -- fallback = "space-nvim",
  },
}
```

**Important:** Setting `lazy = false` and `config = true` is crucial for the
plugin to load your saved colorscheme when Neovim starts.

# ⚙️ Configuration

Configuration is optional. The plugin comes with sensible defaults.

You can customize its behavior by passing an `opts` table to the `setup()` function
or directly in your `lazy.nvim` specification as shown above.

### Default Configuration

Here is the default configuration used by the plugin:

```lua
{
  -- Absolute path to the file where the selected colorscheme name is saved.
  file_path = vim.fn.stdpath("data") .. "/.nvim.colorscheme-persist.lua",

  -- The colorscheme to use if the saved file doesn't exist or is invalid.
  fallback = "default",

  -- A list of colorscheme names to exclude from the Telescope picker.
  -- Those are quite ugly:
  disable = {
    "darkblue", "default", "delek", "desert", "elflord", "evening",
    "industry", "koehler", "morning", "murphy", "pablo", "peachpuff",
    "ron", "shine", "slate", "torte", "zellner"
  },

  -- Options passed directly to the Telescope picker.
  -- See `:help telescope.themes` for more options.
  picker_opts = require("telescope.themes").get_dropdown(),
}
```

### Example Custom Configuration

Here's how you might override some defaults in your `lazy.nvim` setup:

```lua
{
  "propet/colorscheme-persist.nvim",
  lazy = false,
  config = true,
  dependencies = {
    "nvim-telescope/telescope.nvim",
    -- colorschemes
    "folke/tokyonight.nvim"
    "Th3Whit3Wolf/space-nvim",
  },
  opts = {
    -- Save the colorscheme file in the config directory instead
    file_path = vim.fn.stdpath("config") .. "/.current_colorscheme.lua",

    -- Use 'space-nvim' if no scheme is saved yet
    fallback = "space-nvim",

    -- Exclude some specific colorschemes
    disable = { "default", "ron" },

    -- Use a different Telescope theme/layout
    picker_opts = require("telescope.themes").get_ivy({}),
  },
  keys = {
    {
      "<leader>sc", -- Or your preferred keymap
      function()
        require("colorscheme-persist").picker()
      end,
      mode = "n",
      desc = "Choose colorscheme",
    },
  },
}
```

# 🚀 Usage

1.  **Automatic Loading:** If configured with `lazy = false`, the plugin automatically attempts to load and apply the colorscheme saved in the `file_path` every time Neovim starts. If the file doesn't exist or the colorscheme isn't valid, it uses the `fallback` colorscheme.
2.  **Changing Colorscheme:** Use the keymap you configured (e.g., `<leader>sc` in the examples) to open the Telescope picker. Select a colorscheme and press Enter. The chosen colorscheme will be applied immediately and saved to the `file_path` for future sessions.
