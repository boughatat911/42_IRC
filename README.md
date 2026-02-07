# ft_irc

A lightweight **IRC (Internet Relay Chat) server** written in C++. Connect with any IRC client, register with a password, and chat—with a built-in bot for fun commands.

---

## Features

- **TCP server** using `poll()` for non-blocking I/O
- **Client registration**: PASS → NICK → USER (IRC-style)
- **Password protection**: server requires a password to connect
- **Nickname** validation (letters only) and uniqueness checks
- **Built-in bot** with help, time, user info, and other commands
- **Configurable port** (1–65535) and max clients (10)

---

## Requirements

- **C++** compiler (e.g. `g++`, `clang++`)
- **Unix-like** system (Linux, macOS) with POSIX sockets and `poll`

---

## Build

```bash
make
```

Produces the `ircserv` binary.

```bash
make clean    # remove object files
make fclean   # remove objects and binary
make re       # fclean + build
```

---

## Usage

```bash
./ircserv <PORT> <PASSWORD>
```

| Argument   | Description                          |
|-----------|--------------------------------------|
| `PORT`    | Listening port (1–65535), digits only |
| `PASSWORD`| Server password (digits only)        |

**Example:**

```bash
./ircserv 6667 123
```

Then connect with an IRC client (e.g. [irssi](https://irssi.org/), [WeeChat](https://weechat.org/), or `nc`) to `localhost:6667`.

---

## Registration (required)

Connect first, then send in order:

1. **PASS** \<password\>  
   Server password (must be first).

2. **NICK** \<nickname\>  
   Your nickname (letters only).

3. **USER** \<username\> \<hostname\> \<servername\> **:**\<realname\>  
   Username and real name.

**Example (with netcat):**

```bash
nc localhost 6667
PASS 123
NICK Alice
USER guest 0 * :Alice Smith
```

After that you receive the welcome message and can use other commands.

---

## Commands

### Registration & basics
| Command | Description |
|--------|-------------|
| `PASS <password>` | Set server password (first command) |
| `NICK <nickname>` | Set nickname (letters only) |
| `USER <user> <host> <server> :<realname>` | Set username and real name |
| `QUIT :<reason>` | Disconnect from server |

### Bot commands (after registration)
| Command | Description |
|--------|-------------|
| `!HELP` | Show help (registration + commands) |
| `!TIME` | Server time |
| `!DATE` | Server date |
| `!USER <nick>` | Info about a user (nick, username, IP) |
| `!SERVER` | Server info |
| `!ABOUT` | About the server |
| `!RULES` | Server rules |
| `!BATTLE [nick]` | Random fight result |

*(Additional channel commands such as JOIN, PRIVMSG, KICK, INVITE, TOPIC, MODE may be documented in the subject or implemented in a later version.)*

---

## Project structure

```
.
├── main.cpp              # Entry point, port/password parsing
├── Makefile
├── includes/
│   ├── Server.hpp        # Server, poll loop, client array
│   ├── Client.hpp        # Client state (fd, nick, user, buffer)
│   └── Channel.hpp       # Channel (for future use)
├── srcs/
│   ├── Server.cpp        # Socket setup, accept, poll, handle_ClientData
│   ├── Client.cpp        # Client getters/setters, buffer
│   ├── Channel.cpp       # Channel logic
│   └── Bot.cpp           # Bot commands (!HELP, !TIME, !USER, etc.)
└── tools/
    ├── Server_tools.cpp  # processCommand, PASS/NICK/USER/QUIT, helpers
    ├── Client_tools.cpp  # Client utilities (parsing, IP, etc.)
    └── tools.cpp         # Shared helpers (split, ft_toupper, etc.)
```

---

## Connect with an IRC client

- **irssi:** `/connect localhost 6667` then `/quote PASS 123`, set nick and user as needed.
- **WeeChat:** `/server add ft_irc localhost/6667`, set password, then connect and register.
- **Netcat:** See the registration example above.

---

## License

This project was made for educational purposes (e.g. 42 school). Use and modify as allowed by your course rules.
