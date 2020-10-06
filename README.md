# Makefile.lenv

**Makefile.lenv** installs and manages [Lua](https://lua.org) environments installed by [hererocks](https://github.com/mpeterv/hererocks). It depends on and is inspired by [Makefile.venv](https://github.com/sio/Makefile.venv).

## Getting Started

Copy [Makefile.lenv](Makefile.lenv) into your project directory. In your `Makefile`, add your configuration followed by an [include](https://www.gnu.org/software/make/manual/html_node/Include.html)

```make
# Makefile.lenv configuration
ROCKS_TO_INSTALL = ldoc luajson
LUA_VERSION = 5.1.4
LUAROCKS_VERSION = latest

include Makefile.lenv

# ...

include Makefile.venv
```

You will also need to include [Makefile.venv](https://github.com/sio/Makefile.venv) *after* Makefile.lenv, as `$(VENV)/python` is ran to invoke hererocks. In your `REQUIREMENTS_TXT` variable used by Makefile.venv, you should add `hererocks` as Makefile.lenv will invoke it to install the Lua enviornment.

## Configuration

The following variables can be configured within your Makefile in order to control how Makefile.lenv manages your Lua environment:

  * `LENV_DIR`: The directory to which `hererocks` will install the Lua environment. Default: `./.lua`
     * You should not check this file into source control, as it will contain binaries built by hererocks as well as LuaRocks packages.
  * `LUA_VERSION`: Passed to `hererocks` to setup the Lua environment. Default: `latest`
  * `LUAROCKS_VERSION`: Passed to `hererocks` to setup the Lua environment. Default: `latest`
  * `ROCKS_TO_INSTALL`: List of [LuaRocks](https://luarocks.org/) rocks to install in the Lua environment.

## Targets

You can read about all the targets in detail within [Makefile.lenv](Makefile.lenv) comments.

  * `lua`: Starts Lua interactive prompt in the environment
  * `lenv`: Use as dependency for a target that requires the Lua environment to be created and configured in whole.
  * `lua-version`: Displays Lua version in use
  * `luarocks-version` Displays LuaRocks version in use
  * `lenv-reqs`: For targets that require the Lua environment rocks (`$(ROCKS_TO_INSTALL)`) to be installed
  * `clean-lenv`: For targets that require a clean Lua environment directory (removes `LENV_DIR`)

## Available Variables

Use these in your make targets which require `lenv` (see above)

  * `$(LUA)`: Path to Lua executable.
  * `$(LUAROCKS)`: Path to LuaRocks executable (on Windows, batch file)

## Implementation Details

Makefile.lenv uses a few files to keep track of the state of the Lua environment. You can disregard these

  * `$(LENV_DIR)/lua-is-installed.txt`: Indicates that Lua is installed to the Lua environment
  * `$(LENV_DIR)/luarocks-is-installed.txt`: Indicates that LuaRocks is installed to the Lua environment
  * `$(LENV_DIR)/requirements-are-installed.txt`: Indicates that the rocks (LuaRocks dependencies) are installed

