# PyLegiTyper

A Python library for simulating realistic, human-like keyboard and mouse input on Windows systems.

## Overview

PyLegiTyper provides a comprehensive interface for controlling keyboard and mouse input using the Windows API. It includes features for simulating realistic typing patterns with natural delays, mistakes, and corrections - making automated input appear more human-like.

## Features

### Keyboard Control
- **Key Press/Release**: Individual control of any keyboard key
- **Key State Detection**: Check if a key is currently pressed
- **Realistic Typing Simulation**: Simulates human typing patterns including:
  - Variable typing speed (WPM)
  - Random typing mistakes (neighbor keys, double presses, transpositions)
  - Natural delays and pauses
  - Authentic correction behaviors

### Mouse Control
- **Cursor Position**: Get and set cursor coordinates
- **Mouse Buttons**: Press and release any mouse button
- **Mouse Scrolling**: Scroll in any direction (up, down, left, right)

## Installation

### Prerequisites
- Windows operating system
- Python 3.x
- pywin32 library

```bash
pip install pywin32
```

## Usage

### Basic Keyboard Operations

```python
from typer import Keyboard

# Press and release a key
Keyboard.pressAndReleaseKey("a")

# Hold a key
Keyboard.pressKey("shift")
Keyboard.pressKey("a")
Keyboard.releaseKey("a")
Keyboard.releaseKey("shift")

# Type a string
Keyboard.keyboardWrite("Hello, World!")

# Check key state
is_pressed = Keyboard.getKeyState("ctrl")
```

### Realistic Typing Simulation

```python
from typer import Typer

# Simulate typing at 60 WPM with human-like behavior
text = "This text will be typed with realistic delays and occasional mistakes."
Typer.legitTyper(text, wpm=60)
```

### Mouse Operations

```python
from typer import Keyboard

# Get cursor position
x, y = Keyboard.locateCursor()

# Move cursor
Keyboard.moveCursor(500, 300)

# Click mouse buttons
Keyboard.pressAndReleaseMouse("left_mouse")
Keyboard.pressAndReleaseMouse("right_mouse")

# Scroll
Keyboard.scrollMouse("up", 120)
Keyboard.scrollMouse("down", 120)
```

### Supported Keys

The library supports a wide range of keys including:
- **Alphanumeric**: a-z, 0-9
- **Function keys**: F1-F24
- **Control keys**: Enter, Shift, Ctrl, Alt, Tab, Escape, etc.
- **Arrow keys**: Up, Down, Left, Right
- **Media keys**: Volume, Play/Pause, Next, Previous
- **Mouse buttons**: Left, Right, Middle, Button1, Button2
- **Numpad keys**: 0-9, operators
- **Special characters**: Most punctuation and symbols

See `vk_codes` dictionary in the code for complete list.

## Architecture

### Keyboard Class
The main `Keyboard` class provides low-level access to Windows input APIs through ctypes. It includes:
- Direct Windows API bindings using ctypes
- Virtual key code mappings
- C structure definitions for input events
- Error handling utilities

### Typer Class
The `Typer` class provides high-level typing simulation:
- Human-like typing patterns with configurable WPM
- Random typing mistakes with probability-based distribution:
  - Neighbor key mistakes (~6% probability)
  - Double key presses (~3% probability)
  - Character transposition (~2% probability)
- Natural pauses and hesitations
- Automatic mistake correction

### Performance Optimizations

Recent improvements include:
- **Cached neighbor map**: Keyboard layout neighbor relationships are computed once and cached
- **Optimized dictionary lookups**: Reduced redundant hash table lookups
- **Streamlined validation**: Simplified type checking and error handling
- **Efficient random generation**: Optimized delay calculations

## Interactive Mode

The library includes an interactive CLI mode:

```bash
python typer.py
```

This starts a REPL where you can:
1. Type any text
2. The program waits 5 seconds
3. Types the text with realistic human-like behavior
4. Type "exit" or "quit" to close

## Technical Details

### Windows API Integration
- Uses `ctypes.windll.user32` for SendInput and cursor operations
- Implements `KEYBDINPUT` and `MOUSEINPUT` structures
- Supports both virtual key codes and scan codes
- Handles extended keys properly

### Typing Simulation Algorithm
1. Calculates base delay from WPM (Words Per Minute)
2. Applies Gaussian distribution for natural variation
3. Introduces contextual delays:
   - Longer pauses after punctuation
   - Brief thinking pauses after spaces
   - Hesitation after mistakes
4. Simulates common typing errors:
   - Adjacent key mistakes (based on physical keyboard layout)
   - Double key presses
   - Character transpositions

### Error Handling
The library includes comprehensive error checking:
- Type validation for all parameters
- Valid key code verification
- Runtime error reporting
- Graceful error messages

## Limitations

- **Windows Only**: Uses Windows-specific APIs
- **Administrator Rights**: Some operations may require elevated privileges
- **Active Window**: Input is sent to the currently active window
- **Character Support**: Limited to characters in the `vk_codes` mapping

## Code Structure

```
typer.py
├── Keyboard (Class)
│   ├── Constants (VK codes, event flags)
│   ├── C Structures (MOUSEINPUT, KEYBDINPUT, INPUT)
│   ├── Helper Methods (_lookup, _checkCount)
│   ├── Mouse Methods (moveCursor, scrollMouse, pressMouse, etc.)
│   ├── Keyboard Methods (pressKey, releaseKey, keyboardWrite, etc.)
│   └── ManipulateMouse (Nested class for cursor operations)
└── Typer (Class)
    ├── _get_neighbor_map() - Cached keyboard layout
    └── legitTyper() - Human-like typing simulation
```

## Contributing

When contributing, please ensure:
- Code follows existing style patterns
- Type hints are provided for all functions
- Error handling is comprehensive
- Performance optimizations are documented

## License

This project is provided as-is for educational and automation purposes.

## Safety Notice

This library allows programmatic control of keyboard and mouse input. Use responsibly and ethically:
- Do not use for malicious automation
- Respect application terms of service
- Be aware of security implications
- Test in safe environments first

## Future Improvements

Potential enhancements:
- Cross-platform support (Linux, macOS)
- Configurable mistake patterns
- Learning from user typing patterns
- Replay functionality
- Macro recording
- GUI for easier configuration
