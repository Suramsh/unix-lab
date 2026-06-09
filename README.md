[README.md](https://github.com/user-attachments/files/28752936/README.md)
# Unix Lab — Complete Step by Step Reference

## 🔗 [OPEN THE SITE → suramsh.github.io/unix-lab](https://suramsh.github.io/unix-lab)

---

## HOW TO USE THIS
1. Open terminal
2. Follow each step exactly — copy paste the code
3. Run the execute command at the end

---

## CHIT 1 — Vim Editor

### 1a — Vim Operations on test1.txt

**Step 1 — Open file:**
```
vi test1.txt
```
Type the given content, then press `ESC`

**Step 2 — Save:**
```
:wq
```

**Step 3 — Write line 10 to test2.txt:**
```
:10w test2.txt
```

**Step 4 — Replace AI interactively:**
```
:1,$ s/AI/Artificial Learning/gc
```
Press `y` for each match

**Step 5 — Delete line 5 to buffer a:**
```
5G
"add
:w
```

**Step 6 — Switch to test2.txt, paste, switch back:**
```
:e test2.txt
"ap
:e test1.txt
```

**Step 7 — Line 1 → 15 right → 3 down → 2 left → replace with M:**
```
1G
15l
3j
hh
rM
:wq
```

**Step 8 — In test2.txt: 3 words fwd, 2 back, delete, line ends:**
```
:e test2.txt
3w
2b
dw
0
$
:wq
```

**Step 9 — Join first 3 lines:**
```
:e test1.txt
1G
2J
```

**Step 10 — Delete line 7 and quit:**
```
7G
dd
:wq
```

**Output:**
```
replace with Artificial Learning (y/n/a/q)? y
replace with Artificial Learning (y/n/a/q)? y
```

---

### 1b — Correct sample.c in Vim

**Step 1 — Open:**
```
vi sample.c
```

**Step 2 — Type the WRONG code first:**
```c
#include<stio.h>
#include errno.h
int test(int * message)
{print("Errno is %8d',errno) exit;
```

**Step 3 — Fix it to correct code:**
```c
#include <stdio.h>
#include <stdlib.h>
#include <errno.h>
void test(char *message) {
    printf("Errno is %8d\n", errno);
    exit(1);
}
int main() {
    test("Some message");
    return 0;
}
```

**Step 4 — Compile and run:**
```
cc sample.c
./a.out
```

**Output:**
```
Errno is        0
```

---

## CHIT 2 — Shell Scripts

### 2a — Create 10 Departments with 10 Files Each

**Step 1 — Open:**
```
vi prog2a.sh
```

**Step 2 — Type this code:**
```bash
#!/bin/bash
departments="CS IS AI ML CyberSec EC Mechanical EEE DS Civil"
for dept in $departments; do
    mkdir -p "$dept"
    for j in $(seq -f "%03g" 1 10); do
        touch "$dept/name_date.txt-$j"
    done
done
tree
```

**Step 3 — Save and run:**
```
:wq
sh prog2a.sh
```

**Output:**
```
.
|-- CS
|   |-- name_date.txt-001
|   |-- name_date.txt-002
|   |-- ... (up to 010)
|-- IS
|   |-- name_date.txt-001 ... 010
|-- AI
|-- ML
|-- CyberSec
|-- EC
|-- Mechanical
|-- EEE
|-- DS
`-- Civil
    `-- name_date.txt-010
10 directories, 100 files
```

---

### 2b — Display File Permissions and Time

**Step 1 — Open:**
```
vi prog2b.sh
```

**Step 2 — Type this code:**
```bash
#!/bin/bash
if [ $# -eq 0 ]; then
    echo "Usage: sh prog2b.sh <filename>"
else
    ls -l "$1"
fi
```

**Step 3 — Save and run:**
```
:wq
sh prog2b.sh t2
```

**Output:**
```
-rw-rw-r-- 1 rit-admin rit-admin 45 Sep 24 11:43 t2
```

---

## CHIT 3 — Shell Scripts

### 3a — Find Largest File in Directory

**Step 1 — Open:**
```
vi prog3a.sh
```

**Step 2 — Type this code:**
```bash
#!/bin/bash
for i in "$@"; do
    if [ -d "$i" ]; then
        echo "Largest file in $i:"
        ls -Rl "$i" | grep "^-" | tr -s ' ' | \
          cut -d' ' -f5,9 | sort -n | tail -1
    else
        echo "$i is not a directory"
    fi
done
```

**Step 3 — Save and run:**
```
:wq
sh prog3a.sh cy
```

**Output:**
```
Largest file in cy:
135 11:39
```

---

### 3b — Rename File if Exists, Create if Not

**Step 1 — Open:**
```
vi prog3b.sh
```

**Step 2 — Type this code:**
```bash
#!/bin/bash
file_name="example.txt"
if [ -e "$file_name" ]; then
    echo "File '$file_name' exists."
    mv "$file_name" "${file_name}_old"
    echo "Renamed to '${file_name}_old'"
    touch "$file_name"
    echo "New empty file created: '$file_name'"
    echo "Both conditions done together."
else
    touch "$file_name"
    echo "File did not exist. New empty file created."
fi
echo "Script execution completed."
```

**Step 3 — Save and run:**
```
:wq
sh prog3b.sh
```

**Output (first run):**
```
File did not exist. New empty file created.
Script execution completed.
```

**Output (second run):**
```
File 'example.txt' exists.
Renamed to 'example.txt_old'
New empty file created: 'example.txt'
Both conditions done together.
Script execution completed.
```

---

## CHIT 4 — Shell Scripts

### 4a — Disk Space Check

**Step 1 — Open:**
```
vi prog4a.sh
```

**Step 2 — Type this code:**
```bash
#!/bin/bash
space_usage=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')
echo "System Space Usage: $space_usage%"

if [ "$space_usage" -gt 80 ]; then
    echo "Low System Space"
    echo "Files larger than 1GB:"
    find / -type f -size +1G -exec ls -lh {} \; 2>/dev/null
fi
```

**Step 3 — Save and run:**
```
:wq
sh prog4a.sh
```

**Output:**
```
System Space Usage: 62%
```

---

### 4b — Count Words, Characters, Spaces, Special Symbols

**Step 1 — Open:**
```
vi prog4b.sh
```

**Step 2 — Type this code:**
```bash
#!/bin/bash
count_text_stats() {
    input_text="$1"
    char_count=$(echo -n "$input_text" | wc -m)
    word_count=$(echo "$input_text" | wc -w)
    space_count=$(echo "$input_text" | tr -cd ' ' | wc -c)
    special_count=$(echo "$input_text" | tr -cd '[:punct:]' | wc -c)
    echo "Character count: $char_count"
    echo "Word count: $word_count"
    echo "White space count: $space_count"
    echo "Special symbol count: $special_count"
}

text_to_analyze="This is an example text! It contains special symbols, such as @ and #."
count_text_stats "$text_to_analyze"
echo "Script execution completed."
```

**Step 3 — Save and run:**
```
:wq
sh prog4b.sh
```

**Output:**
```
Character count: 70
Word count: 12
White space count: 11
Special symbol count: 5
Script execution completed.
```

---

## CHIT 5 — AWK

### 5a — Date Format Converter

**Step 1 — Open:**
```
vi pgr5.awk
```

**Step 2 — Type this code:**
```awk
#!/usr/bin/awk -f
{
    split($0, arr, "-")
    if (arr[1] < 1 || arr[1] > 31 || arr[2] < 1 || arr[2] > 12) {
        print "Invalid date"
        exit 0
    }
    months = "Jan Feb March April May Jun Jul Aug Sep Oct Nov Dec"
    n = split(months, m, " ")
    print m[arr[2]], arr[1], arr[3]
}
```

**Step 3 — Save and run:**
```
:wq
awk -f pgr5.awk
```
Type `04-09-2024` then press Enter

**Output:**
```
Sep 04 2024
```

---

### 5b — Remove Duplicate Lines

**Step 1 — Open:**
```
vi prog5b.awk
```

**Step 2 — Type this code:**
```awk
#!/usr/bin/awk -f
BEGIN { print "Removing duplicated lines" }
{
    line[++no] = $0
}
END {
    for (i = 1; i <= no; i++) {
        flag = 1
        for (j = 1; j < i; j++)
            if (line[i] == line[j]) flag = 0
        if (flag == 1)
            print line[i] >> "out13a.txt"
    }
}
```

**Step 3 — Save and run:**
```
:wq
awk -f prog5b.awk input1.txt
cat out13a.txt
```

**OR just use this one-liner:**
```
awk '!seen[$0]++' input1.txt > out13a.txt
cat out13a.txt
```

**Output:**
```
Removing duplicated lines

hello
good
```

---

## CHIT 6 — SED & find

### 6a — SED Operations

**Step 1 — Create 1.txt first:**
```
cat > 1.txt
```
Type the content, then press `Ctrl+D`

**Step 2 — Run each SED command:**

```bash
# Task 1 — Replace Python with Perl in line 2 ONLY:
sed '2 s/Python/Perl/g' 1.txt

# Task 2 — Replace LAST occurrence of Programming with Scripting:
sed 's/\(.*\)Programming/\1Scripting/' 1.txt

# Task 3 — Strip path, keep just filename:
echo "/MSRIT/CSE/UG/Python.txt" | sed 's|.*/||'

# Task 4 — Add Learn before and Programming after Bash:
sed 's/\(Bash\)/Learn \1 Programming/' 1.txt
```

**Output Task 1:**
```
Python is a very popular language.
Perl is easy to use. Perl is easy to learn...
```

**Output Task 3:**
```
Python.txt
```

**Output Task 4:**
```
Learn Bash Programming
```

---

### 6b — find Commands

**Run each directly in terminal:**

```bash
# i. Find files with 777 permissions:
find . -type f -perm 0777

# ii. Assign sticky bit to all files:
find . -type f -exec chmod +t {} +

# iii. Find dirs with 777, change to 755:
find . -type d -perm 777 -exec chmod 755 {} +

# iv. Files modified in last 20 days:
find . -type f -mtime -20

# iv. Files accessed in last 20 days:
find . -type f -atime -20

# v. Files modified in last 1 hour:
find . -type f -mmin -60
```

**Output:**
```
$ find . -type f -perm 0777
./rit2/pgm.sh
./cy/66.txt

$ find . -type f -mtime -20
./prog2a.sh
./1.txt
```

---

## CHIT 7 — C Programs

### 7a — Emulate ls -li

**Step 1 — Open:**
```
vi prog7.c
```

**Step 2 — Type this code:**
```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <dirent.h>
#include <unistd.h>
#include <pwd.h>
#include <grp.h>
#include <time.h>

void print_permissions(struct stat fileStat) {
    printf((S_ISDIR(fileStat.st_mode)) ? "d" : "-");
    printf((fileStat.st_mode & S_IRUSR) ? "r" : "-");
    printf((fileStat.st_mode & S_IWUSR) ? "w" : "-");
    printf((fileStat.st_mode & S_IXUSR) ? "x" : "-");
    printf((fileStat.st_mode & S_IRGRP) ? "r" : "-");
    printf((fileStat.st_mode & S_IWGRP) ? "w" : "-");
    printf((fileStat.st_mode & S_IXGRP) ? "x" : "-");
    printf((fileStat.st_mode & S_IROTH) ? "r" : "-");
    printf((fileStat.st_mode & S_IWOTH) ? "w" : "-");
    printf((fileStat.st_mode & S_IXOTH) ? "x" : "-");
}

void list_directory(const char *dirpath) {
    DIR *dir;
    struct dirent *entry;
    struct stat fileStat;
    char fullpath[1024];

    dir = opendir(dirpath);
    if (dir == NULL) { perror("opendir"); exit(EXIT_FAILURE); }

    while ((entry = readdir(dir)) != NULL) {
        snprintf(fullpath, sizeof(fullpath), "%s/%s", dirpath, entry->d_name);
        if (stat(fullpath, &fileStat) == -1) { perror("stat"); continue; }

        printf("%ld ", (long)fileStat.st_ino);
        print_permissions(fileStat);
        printf(" %ld ", (long)fileStat.st_nlink);
        printf("%s %s ", getpwuid(fileStat.st_uid)->pw_name,
                         getgrgid(fileStat.st_gid)->gr_name);
        printf("%5ld ", (long)fileStat.st_size);

        char timebuf[80];
        struct tm *timeinfo = localtime(&fileStat.st_mtime);
        strftime(timebuf, sizeof(timebuf), "%b %d %H:%M", timeinfo);
        printf("%s %s\n", timebuf, entry->d_name);
    }
    closedir(dir);
}

int main(int argc, char *argv[]) {
    const char *dirpath = (argc > 1) ? argv[1] : ".";
    list_directory(dirpath);
    return 0;
}
```

**Step 3 — Save, compile and run:**
```
:wq
cc prog7.c
./a.out
```

**Output:**
```
6074738 -rw-rw-r-- 1 rit-admin rit-admin    12 Sep 25 14:11 create.sh
6073890 -rw-rw-r-- 1 rit-admin rit-admin    39 Sep 24 13:05 prog3c.sh
6029383 drwxr-xr-x 2 rit-admin rit-admin  4096 Jun 03 12:44 Documents
```

---

### 7b — Remove Empty Files from Directory

**Step 1 — Open:**
```
vi prog7b.c
```

**Step 2 — Type this code:**
```c
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>
#include <sys/stat.h>
#include <unistd.h>

void removeEmptyFiles(char *path) {
    DIR *dir;
    struct dirent *entry;
    struct stat fileStat;

    if ((dir = opendir(path)) == NULL) {
        perror("Error opening directory");
        exit(EXIT_FAILURE);
    }

    while ((entry = readdir(dir)) != NULL) {
        char filePath[1024];
        snprintf(filePath, sizeof(filePath), "%s/%s", path, entry->d_name);

        if (stat(filePath, &fileStat) < 0) {
            perror("Error getting file status");
            exit(EXIT_FAILURE);
        }

        if (S_ISREG(fileStat.st_mode) && fileStat.st_size == 0) {
            if (unlink(filePath) == 0)
                printf("Removed empty file: %s\n", entry->d_name);
            else
                perror("Error removing file");
        }
    }
    closedir(dir);
}

int main(int argc, char *argv[]) {
    if (argc != 2) {
        fprintf(stderr, "Usage: %s <directory>\n", argv[0]);
        exit(EXIT_FAILURE);
    }
    removeEmptyFiles(argv[1]);
    return 0;
}
```

**Step 3 — Save, compile and run:**
```
:wq
gcc prog7b.c -o ref
./ref .
```

**Output:**
```
Removed empty file: emptyfile1.txt
Removed empty file: emptyfile2.txt
```

---

## CHIT 8 — C Programs

### 8a — Read n Chars and Append Back (dup)

**Step 1 — Open:**
```
vi prog8a.c
```

**Step 2 — Type this code:**
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#define MAX_SIZE 100

int main() {
    char filename[] = "file.txt";
    int fd, new_fd;
    char buffer[MAX_SIZE];
    ssize_t bytes_read;
    int n;

    printf("Enter the number of characters to read and append: ");
    scanf("%d", &n);

    fd = open(filename, O_RDWR);
    if (fd == -1) { perror("Error opening file"); exit(EXIT_FAILURE); }

    lseek(fd, 0, SEEK_END);
    new_fd = dup(fd);
    if (new_fd == -1) { perror("dup failed"); close(fd); exit(EXIT_FAILURE); }

    lseek(fd, -n, SEEK_END);
    bytes_read = read(fd, buffer, n);
    if (bytes_read == -1) { perror("read failed"); exit(EXIT_FAILURE); }

    if (write(new_fd, buffer, bytes_read) != bytes_read) {
        perror("Error writing");
        close(fd); close(new_fd);
        exit(EXIT_FAILURE);
    }

    close(fd);
    close(new_fd);
    printf("%d characters read and appended successfully.\n", (int)bytes_read);
    return 0;
}
```

**Step 3 — Save, compile and run:**
```
:wq
gcc prog8a.c -o prog8a
echo 'Hello World' > file.txt
./prog8a
```
Enter `5` when prompted

**Output:**
```
Enter the number of characters to read and append: 5
5 characters read and appended successfully.

$ cat file.txt
Hello WorldWorld
```

---

### 8b — List Files in Current Directory

**Step 1 — Open:**
```
vi prog8b.c
```

**Step 2 — Type this code:**
```c
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>

void list_files(const char *dirpath) {
    DIR *dir;
    struct dirent *entry;

    dir = opendir(dirpath);
    if (dir == NULL) { perror("opendir"); exit(EXIT_FAILURE); }

    while ((entry = readdir(dir)) != NULL) {
        if (entry->d_name[0] == '.') {
            continue;
        }
        printf("%s\n", entry->d_name);
    }
    closedir(dir);
}

int main() {
    printf("Files in the current directory:\n");
    list_files(".");
    return 0;
}
```

**Step 3 — Save, compile and run:**
```
:wq
cc prog8b.c
./a.out
```

**Output:**
```
Files in the current directory:
create.sh
prog3c.sh
1.txt
Documents
```

---

## CHIT 9 — C Programs

### 9a — Simulate cp Command

**Step 1 — Open:**
```
vi prog9a.c
```

**Step 2 — Type this code:**
```c
#include <stdio.h>
#include <stdlib.h>
#define BUFFER_SIZE 1024

int main(int argc, char *argv[]) {
    if (argc != 3) {
        fprintf(stderr, "Usage: %s <source_file> <destination_file>\n", argv[0]);
        exit(EXIT_FAILURE);
    }

    FILE *source_file, *destination_file;
    char buffer[BUFFER_SIZE];
    size_t bytesRead;

    source_file = fopen(argv[1], "rb");
    if (source_file == NULL) {
        perror("Error opening source file");
        exit(EXIT_FAILURE);
    }

    destination_file = fopen(argv[2], "wb");
    if (destination_file == NULL) {
        perror("Error opening destination file");
        fclose(source_file);
        exit(EXIT_FAILURE);
    }

    while ((bytesRead = fread(buffer, 1, BUFFER_SIZE, source_file)) > 0) {
        fwrite(buffer, 1, bytesRead, destination_file);
    }

    fclose(source_file);
    fclose(destination_file);
    printf("File copied successfully.\n");
    return 0;
}
```

**Step 3 — Save, compile and run:**
```
:wq
gcc prog9a.c -o mycp
echo 'Hello from source' > source.txt
./mycp source.txt dest.txt
cat dest.txt
```

**Output:**
```
File copied successfully.
Hello from source
```

---

### 9b — Simulate ls Command

**Step 1 — Open:**
```
vi prog9b.c
```

**Step 2 — Type this code:**
```c
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>

int main() {
    DIR *dir;
    struct dirent *entry;

    dir = opendir(".");
    if (dir == NULL) {
        perror("Error opening directory");
        exit(EXIT_FAILURE);
    }

    while ((entry = readdir(dir)) != NULL) {
        printf("%s\n", entry->d_name);
    }

    closedir(dir);
    return 0;
}
```

**Step 3 — Save, compile and run:**
```
:wq
cc prog9b.c
./a.out
```

**Output:**
```
.
..
.bashrc
prog9b.c
a.out
source.txt
```

---

## CHIT 10 — C Programs

### 10a — P1 and P2 Communicate via Pipe

**Step 1 — Open:**
```
vi prog10.c
```

**Step 2 — Type this code:**
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#define BUFFER_SIZE 256

int main() {
    int pipe1[2];
    int pipe2[2];
    pid_t pid;

    if (pipe(pipe1) == -1 || pipe(pipe2) == -1) {
        perror("Pipe creation failed");
        exit(EXIT_FAILURE);
    }

    pid = fork();
    if (pid < 0) { perror("Fork failed"); exit(EXIT_FAILURE); }

    if (pid > 0) {  // P1 (parent)
        close(pipe1[0]);
        close(pipe2[1]);

        char input[BUFFER_SIZE];
        printf("Enter a string: ");
        fgets(input, BUFFER_SIZE, stdin);
        input[strcspn(input, "\n")] = 0;

        write(pipe1[1], input, strlen(input) + 1);

        char output[BUFFER_SIZE];
        read(pipe2[0], output, BUFFER_SIZE);
        printf("Concatenated String from P2: %s\n", output);

        close(pipe1[1]);
        close(pipe2[0]);

    } else {  // P2 (child)
        close(pipe1[1]);
        close(pipe2[0]);

        char received[BUFFER_SIZE];
        read(pipe1[0], received, BUFFER_SIZE);

        const char *concat_str = " - Processed by P2";
        char result[BUFFER_SIZE];
        int i = 0, j = 0;

        while (received[i] != '\0') { result[i] = received[i]; i++; }
        while (concat_str[j] != '\0') { result[i++] = concat_str[j++]; }
        result[i] = '\0';

        write(pipe2[1], result, strlen(result) + 1);

        close(pipe1[0]);
        close(pipe2[1]);
        exit(0);
    }
    return 0;
}
```

**Step 3 — Save, compile and run:**
```
:wq
cc prog10.c
./a.out
```
Type `ramaiah` when prompted

**Output:**
```
Enter a string: ramaiah
Concatenated String from P2: ramaiah - Processed by P2
```

---

### 10b — Simulate grep Command

**Step 1 — Open:**
```
vi prog10b.c
```

**Step 2 — Type this code:**
```c
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>
#define BUFFER_SIZE 1024

void grep_pattern(const char *filename, const char *pattern) {
    int fd;
    char buffer[BUFFER_SIZE];
    ssize_t bytesRead;
    char *line;

    fd = open(filename, O_RDONLY);
    if (fd < 0) { perror("Error opening file"); exit(EXIT_FAILURE); }

    while ((bytesRead = read(fd, buffer, sizeof(buffer))) > 0) {
        buffer[bytesRead] = '\0';
        line = strtok(buffer, "\n");

        while (line != NULL) {
            if (strstr(line, pattern) != NULL) {
                printf("%s\n", line);
            }
            line = strtok(NULL, "\n");
        }
    }

    if (bytesRead < 0) perror("Error reading file");
    close(fd);
}

int main(int argc, char *argv[]) {
    if (argc != 3) {
        fprintf(stderr, "Usage: %s <pattern> <filename>\n", argv[0]);
        exit(EXIT_FAILURE);
    }
    grep_pattern(argv[2], argv[1]);
    return 0;
}
```

**Step 3 — Save, compile and run:**
```
:wq
cc prog10b.c
./a.out hello input1.txt
```

**Output:**
```
hello
(prints every line in input1.txt that contains 'hello')
```

---

*MS Ramaiah Institute of Technology | 3rd Sem CSE AI & ML | Unix Lab*
