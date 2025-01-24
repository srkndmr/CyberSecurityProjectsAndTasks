# Friend Bot

**Friend Bot** is an interactive Bash script that acts as a friendly chatbot. It can tell jokes, provide the current time, calculate basic mathematical expressions, and display a help menu.

---

## Project Structure

The project includes the following files:

1. **`friend.sh`**: The primary script that defines the chatbot functionality.
2. **`jokes.txt`**: A text file containing a list of jokes used by the script.

---

## Script Breakdown: `friend.sh`

### 1. Shebang Line
```bash
#!/bin/bash
```
- Specifies that the script should be executed using the Bash shell.

---

### 2. Define the Jokes File
```bash
JOKES_FILE="jokes.txt"
```
- Defines the path to the `jokes.txt` file containing the jokes.
- Makes it easy to update the jokes file path without modifying multiple parts of the script.

---

### 3. Validate Jokes File
```bash
if [ ! -f "$JOKES_FILE" ]; then
    echo "Error: Jokes file ($JOKES_FILE) not found. Please create it with some jokes."
    exit 1
fi
```
- **Purpose**: Ensures the `jokes.txt` file exists before proceeding.
- **Details**:
  - `[ ! -f "$JOKES_FILE" ]`: Checks if the file does not exist.
  - `exit 1`: Exits the script with an error code if the file is missing.

---

### 4. Define Functions

#### a. **Tell a Joke**
```bash
tell_joke() {
    local num_jokes=$(grep -cve '^\s*$' "$JOKES_FILE")
    if [ "$num_jokes" -eq 0 ]; then
        echo "Oops! It seems the jokes file is empty or has only blank lines."
        return
    fi
    local random_line=$(( (RANDOM % num_jokes) + 1 ))
    local joke=$(grep -ve '^\s*$' "$JOKES_FILE" | sed -n "${random_line}p")
    if [ -n "$joke" ]; then
        echo "Here's a joke for you: $joke"
    else
        echo "Oops! I couldn't fetch a joke. Please check the jokes file."
    fi
}
```
- **Purpose**: Selects and prints a random joke from the `jokes.txt` file.
- **Process**:
  1. Counts the jokes, ignoring empty lines.
  2. Generates a random number between 1 and the number of jokes.
  3. Fetches the joke corresponding to the random number.
  4. Outputs the joke or an error message if no joke is found.

---

#### b. **Tell the Current Time**
```bash
tell_time() {
    echo "The current date and time is: $(date)"
}
```
- **Purpose**: Displays the current system date and time.
- **Details**:
  - `$(date)`: Executes the `date` command to fetch the current time.

---

#### c. **Calculate an Expression**
```bash
calculate() {
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
```
- **Purpose**: Evaluates a simple mathematical expression and outputs the result.
- **Details**:
  - Validates the input using `awk`.
  - Uses `bc -l` for precise calculations.

---

#### d. **Display Help**
```bash
show_help() {
    echo "I can do the following:"
    echo "- Type 'tell me a joke' to hear a joke."
    echo "- Type 'what's the time' to see the current time."
    echo "- Enter a simple math expression like '4 + 5' to calculate it."
    echo "- Type 'exit' to leave."
}
```
- **Purpose**: Displays a list of available commands.

---

### 5. Interactive Mode
```bash
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
                calculate "$input"
                ;;
        esac
    done
}
```
- **Purpose**: Engages the user in an interactive session.
- **Process**:
  1. Prompts the user for input.
  2. Matches input against predefined commands.
  3. Calls the appropriate function or attempts to calculate a math expression.

---

### 6. Non-Interactive Mode
```bash
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
            calculate "$1"
            ;;
    esac
}
```
- **Purpose**: Handles single commands passed as arguments to the script.
- **Examples**:
  - `./friend.sh joke` tells a joke.
  - `./friend.sh "4 + 5"` calculates the result.

---

### 7. Main Logic
```bash
if [ -t 0 ]; then
    interactive_mode
else
    non_interactive_mode "$@"
fi
```
- **Purpose**: Determines whether the script runs in interactive or non-interactive mode.
- **Details**:
  - `[ -t 0 ]`: Checks if the script is running in a terminal (interactive mode).
  - Otherwise, it processes command-line arguments.

---

## How to Use Friend Bot

### 1. Prerequisites
- A `jokes.txt` file containing jokes (one per line).

### 2. Make the Script Executable
```bash
chmod +x friend.sh
```

### 3. Run the Script

#### Interactive Mode
```bash
./friend.sh
```
- Engage with the bot directly in the terminal.

#### Non-Interactive Mode
```bash
./friend.sh <command>
```
- Example:
  - `./friend.sh joke`
  - `./friend.sh "4 + 5"`

---

## Example Interaction

### Interactive Mode
```bash
$ ./friend.sh
Hi, I'm your friend! Type 'help' to see what I can do, or 'exit' to leave.
You: tell me a joke
Here's a joke for you: Why don’t eggs tell jokes? They’d crack each other up.
You: what's the time
The current date and time is: Thu Jan 25 10:25:14 UTC 2025
You: help
I can do the following:
- Type 'tell me a joke' to hear a joke.
- Type 'what's the time' to see the current time.
- Enter a simple math expression like '4 + 5' to calculate it.
- Type 'exit' to leave.
You: exit
Goodbye!
```

### Non-Interactive Mode
```bash
$ ./friend.sh joke
Here's a joke for you: Why was the computer cold? It left its Windows open!

$ ./friend.sh "4 * 7"
The result of 4 * 7 is: 28
```

---

## Notes

1. **Dependencies**:
   - No additional dependencies are required.
2. **Error Handling**:
   - Ensures the `jokes.txt` file exists.
   - Validates mathematical expressions before attempting calculations.

---

