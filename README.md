# happy-birthday
Hi, JJ, aka platform engineer, I and ChatGPT crafted a fun, easy yet intricate "Happy Birthday" Capture the Flag (CTF) challenge tailored to your skill set. There is a mix of problem-solving, platform engineering-related concepts, and a few sneaky tricks to keep things fun and engaging.

### Overview of the CTF:
The CTF could be a multi-stage challenge with various layers of puzzles and tasks. Each stage leads to a flag or clue, and the final flag will be a birthday message. We set it up as a series of hidden clues in different formats (e.g., text, encoded strings, simple reverse engineering, and debugging). We’ll use concepts from areas like web security, Linux systems, and containerization—something a platform engineer would be familiar with but still find enjoyable.

### Stage 1: Initial Clue in a Docker Container
**Challenge:** Start with a Docker container that simulates a production environment (since platform engineers often work with containers). The container will have a misconfiguration or hidden files that reveal a clue.

- **Setup:** Create a Docker container that starts a simple web server (e.g., Nginx or Apache). Inside the container, place a hidden file with the next clue. The misconfiguration can be something like exposing the Docker socket or leaving an unnecessary service running (like an SSH server).

- **Clue:** The next flag can be found in `/etc/happy_birthday.txt`.

- **Example Dockerfile:**
    ```Dockerfile
    FROM nginx:latest

    RUN echo "Flag 1: Great! You found this. Now check the /opt directory!" > /etc/happy_birthday.txt

    RUN mkdir -p /opt/hidden_directory && echo "Flag 2: Time for the next challenge. Decode the following: aHR0cHM6Ly93d3cueW91cnNpdGVoZXJlLmNvbS9iYXNlbGluZQo=" > /opt/hidden_directory/clue.txt

    CMD ["nginx", "-g", "daemon off;"]
    ```

**Hint for your friend:** They can either enter the running container using `docker exec` or use other methods like inspecting the container image with `docker history`.

---

### Stage 2: Base64 Encoding & Hidden Clue
**Challenge:** In this stage, the hidden clue in `/opt/hidden_directory/clue.txt` is Base64 encoded. When decoded, it leads them to a URL with the next flag.

- **Clue:** The Base64 string `aHR0cHM6Ly93d3cueW91cnNpdGVoZXJlLmNvbS9iYXNlbGluZQo=` decodes to `https://www.yoursitehere.com/baseline`.

- **Task:** After decoding, they should visit the URL, which can be a simple webpage containing the next flag or an additional puzzle.

**Webpage (hosted on your web server):**
```html
<html>
<head><title>Flag 3</title></head>
<body>
    <h1>Happy Birthday! But we're not done yet.</h1>
    <p>Flag 3: Search for hidden comments in the HTML source!</p>
    <!-- Flag 4: "Well done, now ssh into the server with the key you find next!" -->
</body>
</html>
```

---

### Stage 3: SSH into a Server (Public Key Challenge)
**Challenge:** At this point, you can provide an SSH key for them to access a server (or a simulated environment). The next flag can be hidden within the server.

- **Setup:** On the server, place the next flag in a hidden file or have them traverse the file system to find a clue that leads to the final flag.

- **SSH Setup:**
  - Create a new user for them and give them an SSH private key.
  - Ensure that only the private key you provide grants access.
  
- **Next Clue:** Once inside, they can find a clue in a file like `/home/ctf_user/birthday_flag.txt`.

Example contents:
```bash
Flag 4: Congrats! The final flag is hidden in /var/tmp/surprise.
```

---

### Stage 4: Simple Reverse Engineering
**Challenge:** Inside `/var/tmp/surprise`, place an ELF binary (compiled C code) that they need to reverse engineer to find the final flag.

- **Binary Contents:** You can create a small C program that prints a flag after a certain input or password is provided.
  
- **Simple C Program (Reverse Engineering Puzzle):**
    ```c
    #include <stdio.h>
    #include <string.h>

    int main() {
        char password[20];
        printf("Enter the birthday password: ");
        scanf("%s", password);

        if (strcmp(password, "cakeandcode") == 0) {
            printf("Congrats! Final Flag: HAPPY_BIRTHDAY_PLATFORM_ENGINEER!\n");
        } else {
            printf("Wrong password, try again!\n");
        }

        return 0;
    }
    ```

- **Clue to Solve:** You can reverse engineer this binary using tools like `strings`, `gdb`, or `radare2` to identify the correct input (`cakeandcode`).

---

### Final Stage: Birthday Surprise
Once you enter the correct password, the binary prints out the final birthday message, which could be something like:
```
HAPPY_BIRTHDAY_PLATFORM_ENGINEER!
```

---

### Summary of Stages:
1. **Docker Misconfiguration Clue**: Find the first flag in the Docker container and locate the hidden clue.
2. **Base64 Decoding**: Decode the string to find a URL with the next flag.
3. **SSH Key Challenge**: Use the SSH key to log into a server, where a flag is hidden.
4. **Simple Reverse Engineering**: Reverse engineer a binary to find the final birthday message.
