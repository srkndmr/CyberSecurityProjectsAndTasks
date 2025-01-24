
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

---

## License

Friend Bot is free to use and share. Modify it to add your personalized features or jokes!

