# EX. NO: 2 – IMPLEMENTATION OF PLAYFAIR CIPHER

## AIM

To write a C program to implement the Playfair Substitution technique.

---


## DESCRIPTION

The Playfair Cipher is a classical symmetric substitution cipher that encrypts plaintext in pairs of letters (digraphs) instead of encrypting one letter at a time. It uses a **5 × 5 key matrix** generated from a keyword. Since the English alphabet contains 26 letters, the letters **I** and **J** are treated as the same letter to fit into the 25 cells of the matrix.

To encrypt a message, the plaintext is divided into pairs of letters. If a pair contains the same letter, an **X** is inserted between them. If a single letter remains at the end, an **X** is appended to complete the pair.

Each pair is encrypted using one of the following rules:

1. If both letters are the same (or only one letter is left), insert **X** after the first letter.
2. If both letters are in the same row of the key matrix, replace each letter with the letter immediately to its right.
3. If both letters are in the same column, replace each letter with the letter immediately below it.
4. If the letters are in different rows and columns, form a rectangle and replace each letter with the letter in the same row but in the other letter's column.

---

## EXAMPLE

### Keyword

```
MONARCHY
```

### Key Matrix

| M | O | N | A | R |
|---|---|---|---|---|
| C | H | Y | B | D |
| E | F | G | I/J | K |
| L | P | Q | S | T |
| U | V | W | X | Z |

### Plaintext

```
HELLO
```

### Prepared Plaintext

```
HE LX LO
```

### Encryption

| Plain Pair | Rule Used | Cipher Pair |
|------------|-----------|-------------|
| HE | Rectangle Rule | CF |
| LX | Rectangle Rule | SU |
| LO | Rectangle Rule | PM |

### Ciphertext

```
CFSUPM
```

---

## ALGORITHM

**STEP 1:** Read the plaintext from the user.

**STEP 2:** Read the keyword.

**STEP 3:** Create a **5 × 5 key matrix** by:
- Removing duplicate letters from the keyword.
- Combining **I** and **J** into one cell.
- Filling the remaining cells with unused letters of the alphabet.

**STEP 4:** Prepare the plaintext:
- Convert all letters to uppercase.
- Remove spaces.
- Divide the plaintext into pairs.
- Insert **X** between repeated letters.
- Append **X** if the plaintext length is odd.

**STEP 5:** Encrypt each pair using the Playfair cipher rules:
- Same Row → Move one position to the right.
- Same Column → Move one position down.
- Rectangle Rule → Replace each letter with the letter in the same row but in the opposite column.

**STEP 6:** Display the encrypted ciphertext.

---

## PROGRAM
```
Name : Prahathieswaran S
Register No. 212225240107
```

```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char matrix[5][5];

// Create Playfair Matrix
void createMatrix(char key[])
{
    int used[26] = {0};
    int i, j, k = 0;

    // I and J are treated as same
    used['J' - 'A'] = 1;

    for(i = 0; key[i] != '\0'; i++)
    {
        char ch = toupper(key[i]);

        if(ch == 'J')
            ch = 'I';

        if(ch >= 'A' && ch <= 'Z' && !used[ch - 'A'])
        {
            matrix[k / 5][k % 5] = ch;
            used[ch - 'A'] = 1;
            k++;
        }
    }

    for(i = 0; i < 26; i++)
    {
        if(!used[i])
        {
            matrix[k / 5][k % 5] = i + 'A';
            k++;
        }
    }
}

// Find position of a character
void findPosition(char ch, int *row, int *col)
{
    int i, j;

    if(ch == 'J')
        ch = 'I';

    for(i = 0; i < 5; i++)
    {
        for(j = 0; j < 5; j++)
        {
            if(matrix[i][j] == ch)
            {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

// Prepare plaintext
void prepareText(char text[], char prepared[])
{
    int i, j = 0;

    // Remove spaces and convert to uppercase
    for(i = 0; text[i] != '\0'; i++)
    {
        if(isalpha(text[i]))
        {
            prepared[j++] = toupper(text[i]);
        }
    }

    prepared[j] = '\0';

    char temp[200];
    int k = 0;

    for(i = 0; i < j; i++)
    {
        temp[k++] = prepared[i];

        if(prepared[i] == prepared[i + 1])
        {
            temp[k++] = 'X';
        }
    }

    if(k % 2 != 0)
    {
        temp[k++] = 'X';
    }

    temp[k] = '\0';

    strcpy(prepared, temp);
}

// Encrypt plaintext
void encrypt(char text[])
{
    int i;

    printf("\nCipher Text : ");

    for(i = 0; text[i] != '\0'; i += 2)
    {
        int r1, c1, r2, c2;

        findPosition(text[i], &r1, &c1);
        findPosition(text[i + 1], &r2, &c2);

        // Same Row
        if(r1 == r2)
        {
            printf("%c%c",
                   matrix[r1][(c1 + 1) % 5],
                   matrix[r2][(c2 + 1) % 5]);
        }
        // Same Column
        else if(c1 == c2)
        {
            printf("%c%c",
                   matrix[(r1 + 1) % 5][c1],
                   matrix[(r2 + 1) % 5][c2]);
        }
        // Rectangle Rule
        else
        {
            printf("%c%c",
                   matrix[r1][c2],
                   matrix[r2][c1]);
        }
    }

    printf("\n");
}

int main()
{
    char key[100];
    char plaintext[100];
    char prepared[200];
    int i, j;

    printf("Enter Keyword : ");
    scanf("%s", key);

    printf("Enter Plaintext : ");
    scanf("%s", plaintext);

    createMatrix(key);

    printf("\nPlayfair Matrix\n");

    for(i = 0; i < 5; i++)
    {
        for(j = 0; j < 5; j++)
        {
            printf("%c ", matrix[i][j]);
        }
        printf("\n");
    }

    prepareText(plaintext, prepared);

    printf("\nPrepared Text : %s\n", prepared);

    encrypt(prepared);

    return 0;
}
```

---

## SAMPLE INPUT

```
Enter Keyword : MONARCHY
Enter Plaintext : HELLO
```

---

## SAMPLE OUTPUT

```
Playfair Matrix

M O N A R
C H Y B D
E F G I K
L P Q S T
U V W X Z

Prepared Text : HELXLO

Cipher Text : CFSUPM
```

---

## OUTPUT SCREENSHOT


Example:

<img width="1715" height="618" alt="image" src="https://github.com/user-attachments/assets/2fb31f20-41a8-4f8b-b2d0-885ac71c72b5" />

---

## RESULT

Thus, the C program to implement the **Playfair Substitution Cipher** was successfully executed, and the corresponding ciphertext was generated.
