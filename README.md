# USER.NVIM
Since the advent of vim and neovim, countless package managers have appeared and dissappeared. Well, here's another one, inspired by Emacs' (straight.el) and use-package

## Philosophy
Configurations should be reproducible and require minimal user effort.
With `user.nvim`, there is no need to run something like :PlugInstall. Once your config file is written, you are done!

**NOTE:** You still need to run `require("user").update()` from time to time. Updates are not necesssary for getting your config up and running, and aren't handled automatically.

## Requirements
(Neovim 0.5.0)[https://neovim.io/]
(Git)[https://git-scm.com/]

## Recommendations
Neovim 0.5.0 now supports using init.lua, where lua code can be put.
If you have a init.vim or .vimrc, you can put your lua code in a heredoc block:

```
lua << EOF
-- lua code goes here
EOF
```

## Usage
use {...}
```lua
require("user").use {
	"package_author/package_name",
	disabled = false, -- if true, ignored this use call.
	after = {}, -- list of dependencies, run config() only after these have been loaded
	init = function()
		-- will run immediately unless disabled = true.
	end,
	config = function()
		-- will run immediately after the package is loaded.
	end,
}
```

update
```lua
require("user").update()
```

clean
```lua
require("user").clean()
```
## Example
```lua
-- Bootstrapping
local user_install_path = vim.fn.stdpath("data").."/site/pack/user/opt/faerryn-user.nvim"
if vim.fn.empty(vim.fn.glob(user_install_path)) > 0 then
	os.execute([[git clone --depth 1 https://github.com/faerryn/user.nvim.git ']]..user_install_path..[[']])
end
vim.api.nvim_command("packadd faerryn-user.nvim")

local user = require("user")
user.setup()
local use = user.use

-- user.nvim needs to manage itself!
use "faerryn/user.nvim"

-- Gruvbox is mandatory
use {
	"gruvbox-community/gruvbox",
	config = function()
		vim.api.nvim_command("colorscheme gruvbox")
	end,
}

-- gitsigns.nvim requires plenary.nvim, but plenary.nvim's declaration is 
use {
	"lewis6991/gitsigns.nvim",
	after = "nvim-lua/plenary.nvim",
	config = function()
		require("gitsigns").setup()
	end,
}

use "nvim-lua/plenary.nvim"
```
