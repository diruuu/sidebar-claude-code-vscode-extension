# Claude Code Sidebar Extension for VS Code

This VS Code extension integrates Claude Code directly into your VS Code sidebar, allowing you to interact with Claude Code without leaving your editor.

## Features

- **Sidebar Integration**: Adds a Claude Code icon to the VS Code activity bar
- **Real Terminal**: Opens a real shell terminal (zsh/bash/etc.) in the sidebar with xterm.js
- **Auto-start Claude**: Automatically runs the `claude` command when the terminal opens
- **Auto Cleanup**: Automatically closes the Claude session when VS Code exits or restarts
- **Full Terminal Support**: Full terminal capabilities with proper PTY support using node-pty

## Prerequisites

Before installing this extension, make sure you have:

1. **Node.js** (v18 or higher) - [Download here](https://nodejs.org/)
2. **VS Code** (v1.85.0 or higher) - [Download here](https://code.visualstudio.com/)
3. **Claude Code CLI** installed and accessible in your PATH
   - You can verify this by running `claude --version` in your terminal
   - If not installed, follow the [Claude Code installation guide](https://docs.claude.com/)
4. **Build Tools** (required for node-pty native module):
   - **macOS**: Xcode Command Line Tools - `xcode-select --install`
   - **Windows**: Visual Studio Build Tools (with C++ workload)
   - **Linux**: build-essential - `sudo apt-get install build-essential` (Ubuntu/Debian)

## Installation

### Step 1: Clone or Download the Repository

```bash
git clone https://github.com/yourusername/sidebar-claude-code-vscode-extension.git
cd sidebar-claude-code-vscode-extension
```

### Step 2: Install Dependencies

```bash
npm install
```

**Important:** The extension uses `node-pty`, a native Node.js module that needs to be rebuilt for VS Code's Electron version. The `npm install` command will automatically rebuild it via the postinstall script. If you encounter any issues, manually rebuild with:

```bash
npm run rebuild
```

### Step 3: Compile the Extension

```bash
npm run compile
```

This will compile the TypeScript code to JavaScript in the `out` directory.

### Step 4: Install the Extension Locally

There are two ways to install the extension locally:

#### Option A: Using the Extension Development Host (Recommended for Testing)

1. Open the extension folder in VS Code:
   ```bash
   code .
   ```

2. Press `F5` or go to `Run > Start Debugging`

3. This will open a new VS Code window with the extension loaded

4. In the new window, you should see the Claude Code icon in the activity bar (sidebar)

#### Option B: Package and Install (Recommended for Regular Use)

1. Install the `vsce` tool if you haven't already:
   ```bash
   npm install -g @vscode/vsce
   ```

2. Package the extension:
   ```bash
   vsce package
   ```

   This creates a `.vsix` file (e.g., `claude-code-sidebar-0.0.1.vsix`)

3. Install the packaged extension:
   - Open VS Code
   - Go to Extensions view (`Ctrl+Shift+X` or `Cmd+Shift+X`)
   - Click the `...` menu at the top
   - Select `Install from VSIX...`
   - Choose the `.vsix` file you just created

4. Reload VS Code when prompted

## Usage

### Opening Claude Code

1. Click the Claude Code icon in the activity bar (left sidebar)
2. A terminal will open in the sidebar (your default shell: zsh, bash, etc.)
3. The extension automatically runs `claude` command for you
4. You can now interact with Claude Code directly in the terminal

### Interacting with Claude

- Type directly in the terminal - it's a real terminal with full functionality
- Use all Claude Code commands and features as you normally would
- The terminal supports all standard terminal operations (copy, paste, keyboard shortcuts, etc.)

### Closing Claude Code

The terminal and Claude session will automatically close when:
- You close VS Code
- You reload VS Code window (`Ctrl+R` or `Cmd+R`)
- The extension is deactivated

You can also manually exit Claude by typing `exit` or pressing `Ctrl+D`, then close the sidebar panel.

## Troubleshooting

### Extension doesn't appear in the sidebar

- Make sure the extension is installed correctly
- Try reloading VS Code (`Ctrl+R` or `Cmd+R`)
- Check the Output panel (`View > Output`) and select "Claude Code Sidebar" for error messages

### "Claude is not found" or similar error

- Ensure the `claude` command is in your system PATH
- Try running `claude --version` in your regular terminal
- If it doesn't work, reinstall Claude Code CLI or add it to your PATH

### "node-pty" module errors or NODE_MODULE_VERSION mismatch

This happens when node-pty is compiled for a different Node.js version than VS Code's Electron uses.

**Solution:**
```bash
# Rebuild node-pty for VS Code's Electron version
npm run rebuild

# Or do a fresh install
rm -rf node_modules
npm install
```

The postinstall script should automatically rebuild node-pty, but if it fails:
- Make sure you have build tools installed (Xcode Command Line Tools on macOS, Visual Studio Build Tools on Windows, build-essential on Linux)
- On macOS: `xcode-select --install`
- On Ubuntu/Debian: `sudo apt-get install build-essential`
- On Windows: Install Visual Studio Build Tools

### Terminal shows errors or doesn't respond

- Check if Claude Code CLI is working correctly outside VS Code
- Look at the VS Code Developer Console (`Help > Toggle Developer Tools`) for error messages
- Make sure you have the latest version of Claude Code installed

### Claude process doesn't close properly

- The extension tries to clean up automatically, but if you notice lingering processes:
  - Check running processes: `ps aux | grep claude`
  - Manually kill if needed: `kill <process-id>`

## Development

### Project Structure

```
sidebar-claude-code-vscode-extension/
├── src/
│   └── extension.ts          # Main extension code
├── resources/
│   └── claude-icon.svg        # Extension icon
├── out/                       # Compiled JavaScript (generated)
├── package.json               # Extension manifest
├── tsconfig.json             # TypeScript configuration
└── README.md                 # This file
```

### Making Changes

1. Edit the TypeScript files in `src/`
2. Compile: `npm run compile`
3. Test using `F5` (Extension Development Host)
4. Package and install for production use

### Watch Mode

For active development, you can use watch mode to automatically recompile on changes:

```bash
npm run watch
```

## Configuration

Currently, the extension uses default settings. Future versions may include:
- Custom Claude CLI path
- Terminal appearance settings
- Auto-start preferences

## How It Works

The extension:
1. Creates a webview in the VS Code sidebar
2. Uses **xterm.js** to render a real terminal in the webview
3. Uses **node-pty** to create a PTY (pseudo-terminal) running your default shell
4. Automatically sends the `claude` command to the terminal after it starts
5. Provides full terminal functionality with proper input/output handling
6. Cleans up the PTY process when VS Code exits or the extension deactivates

## Future Enhancements

- [ ] Customizable terminal appearance (colors, fonts)
- [ ] Support for multiple Claude sessions
- [ ] Configuration options for shell and Claude CLI path
- [ ] Persistent session across VS Code reloads (optional)
- [ ] Better error handling and recovery

## License

MIT

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## Support

If you encounter any issues or have questions:
1. Check the Troubleshooting section above
2. Open an issue on GitHub
3. Check the Claude Code documentation

## Changelog

### 0.0.1 (Initial Release)
- Sidebar integration with Claude Code icon
- Real terminal using xterm.js and node-pty
- Auto-start Claude command on terminal launch
- Full PTY support with proper terminal emulation
- Auto cleanup on VS Code exit/restart
- Terminal resize support
