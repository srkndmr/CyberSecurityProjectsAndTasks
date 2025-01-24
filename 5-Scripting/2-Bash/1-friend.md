# Friend Bot

Friend Bot is a Bash-based interactive script that entertains users with jokes, performs calculations, and provides the current time.

## Project Structure

The project consists of the following two files:

1. **`friend.sh`**: The primary script that handles user interactions and commands.
2. **`jokes.txt`**: A text file containing jokes, with one joke per line.

---

## How to Use Friend Bot

### 1. Prerequisites
   - Ensure you have Bash installed (most Unix-based systems come with it pre-installed).
   - Place both `friend.sh` and `jokes.txt` in the same directory.

### 2. Make the Script Executable
   Run the following command to make the script executable:
   ```bash
   chmod +x friend.sh
   ```

### 3. Run the Script
   Start the bot with:
   ```bash
   ./friend.sh
   ```

### 4. Interactive Commands
   While the script is running, you can use the following commands:

   - **`tell me a joke`**: Retrieves a random joke from `jokes.txt`.
   - **`what's the time`**: Displays the current date and time.
   - **Math Expression**: Enter a simple equation like `4 + 5` to calculate it.
   - **`help`**: Shows a list of commands the bot can process.
   - **`exit`**: Exits the program with a goodbye message.

---

## Script Overview

### 1. **Initialization**
The script begins by checking for the presence of `jokes.txt`. If the file is missing, it exits with an error message. This ensures the bot's functionality remains intact.

### 2. **Core Functions**

- **Joke Teller (`tell_joke`)**:
  - Checks if `jokes.txt` exists and contains jokes.
  - Selects a random joke using the `grep` and `sed` commands.

- **Time Teller (`tell_time`)**:
  - Displays the current system date and time using the `date` command.

- **Calculator (`calculate`)**:
  - Evaluates simple mathematical expressions using `awk` and `bc`.

- **Help Display (`show_help`)**:
  - Lists all available commands and their descriptions.

### 3. **Interactive Mode**
The bot runs an infinite loop to listen for user inputs. Based on the input, it calls the corresponding function or performs error handling for unrecognized commands.

### 4. **Non-Interactive Mode**
The script can also process a single command passed as an argument (e.g., `./friend.sh joke`).

---

## Script File: `friend.sh`

```bash
#!/bin/bash

# File containing jokes
JOKES_FILE="jokes.txt"

# Check if the jokes file exists
if [ ! -f "$JOKES_FILE" ]; then
    echo "Error: Jokes file ($JOKES_FILE) not found. Please create it with some jokes."
    exit 1
fi

# Function to tell a random joke
tell_joke() {
    # Count the number of jokes in the file (ignoring empty lines)
    local num_jokes=$(grep -cve '^\s*$' "$JOKES_FILE")
    
    # Check if the file is empty
    if [ "$num_jokes" -eq 0 ]; then
        echo "Oops! It seems the jokes file is empty or has only blank lines."
        return
    fi

    # Generate a random line number and fetch the joke
    local random_line=$(( (RANDOM % num_jokes) + 1 ))
    local joke=$(grep -ve '^\s*$' "$JOKES_FILE" | sed -n "${random_line}p")

    # Check if a joke was retrieved successfully
    if [ -n "$joke" ]; then
        echo "Here's a joke for you: $joke"
    else
        echo "Oops! I couldn't fetch a joke. Please check the jokes file."
    fi
}

# Function to tell the current time
tell_time() {
    echo "The current date and time is: $(date)"
}

# Function to calculate a simple equation
calculate() {
    # Validate input and calculate using awk
    if echo "$1" | awk '/^[0-9.\+\-\*\/() ]+$/ { exit 0 } { exit 1 }'; then
        local result=$(echo "$1" | bc -l 2>/dev/null)
        if [ -n "$result" ]; then
            echo "The result of $1 is: $result"
        else
            echo "I couldn't calculate that. Please try a simple equation like '4 + 5'."
        fi
    else
        echo "I couldn't calculate that. Please try a simple equation like '4 + 5'."
    fi
}

# Function to display help
show_help() {
    echo "I can do the following:"
    echo "- Type 'tell me a joke' to hear a joke."
    echo "- Type 'what's the time' to see the current time."
    echo "- Enter a simple math expression like '4 + 5' to calculate it."
    echo "- Type 'exit' to leave."
}

# Interactive mode
interactive_mode() {
    echo "Hi, I'm your friend! Type 'help' to see what I can do, or 'exit' to leave."
    while true; do
        read -p "You: " input
        case "$input" in
            "tell me a joke")
                tell_joke
                ;;
            "what's the time")
                tell_time
                ;;
            "help")
                show_help
                ;;
            "exit")
                echo "Goodbye!"
                break
                ;;
            *)
                # Try to calculate if input looks like a math expression
                calculate "$input"
                ;;
        esac
    done
}

# Non-interactive mode
non_interactive_mode() {
    case "$1" in
        "joke")
            tell_joke
            ;;
        "time")
            tell_time
            ;;
        "help")
            show_help
            ;;
        *)
            # Assume the input is a mathematical expression
            calculate "$1"
            ;;
    esac
}

# Main script logic
if [ -t 0 ]; then
    # Interactive mode
    interactive_mode
else
    # Non-interactive mode
    non_interactive_mode "$@"
fi
```

---

## Example Interaction

```bash
$ ./friend.sh
Hi, I'm your friend! Type 'help' to see what I can do, or 'exit' to leave.
You: help
I can do the following:
- Type 'tell me a joke' to hear a joke.
- Type 'what's the time' to see the current time.
- Enter a simple math expression like '4 + 5' to calculate it.
- Type 'exit' to leave.
You: tell me a joke
Here's a joke for you: Why don’t eggs tell jokes? They’d crack each other up.
You: what's the time
The current date and time is: Fri Jan 24 15:32:45 UTC 2025
You: 10 / 2
The result of 10 / 2 is: 5
You: exit
Goodbye!
```

---

## Notes

- **File Requirements**:
  - Ensure `jokes.txt` exists in the same directory as `friend.sh`.
  - `jokes.txt` should have at least one joke; each joke should occupy a separate line.

- **Error Handling**:
  - If `jokes.txt` is missing, the bot exits with an error.
  - Invalid commands prompt the user to try again or enter `help` for guidance.



