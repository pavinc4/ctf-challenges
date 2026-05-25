Room Link : [Compiled](https://tryhackme.com/room/compiled)


<img width="493" height="146" alt="Image" src="https://github.com/user-attachments/assets/cbd960e9-8697-49c4-9f3e-e430a6bf9b9c" />

> `reverse engineering`

---

# **Objective**

**The goal of this challenge was to reverse engineer a compiled Linux binary file and extract the correct password.**

> Step 1 - Initial Discovery & Execution

> Step 2 - Decompilation & Logic Analysis

> Step 3 - Finding the Password

---

## Step 1 : Initial Discovery & Execution

Before diving into decompilation, I ran the `file` command to check the binary's basic properties:

<img width="1365" height="138" alt="Image" src="https://github.com/user-attachments/assets/4cbf3afa-f7ef-4be5-847f-8d18e300fb3e" />

- **ELF 64-bit LSB pie executable:** Confirmed the file is a standard 64-bit Linux executable using Position Independent Executable (PIE) security.
- **Not Stripped:** This means the binary still has its original function names (like `main`), which makes it much easier to reverse engineer in Ghidra.

I checked the properties of the file and executed it to see how it behaves

<img width="612" height="127" alt="Image" src="https://github.com/user-attachments/assets/a92ce42c-3c9d-4ba6-aec1-faed0fd80210" />

- You can see, The program directly asks for a `Password:`
- Entering a dummy value (`test123`) triggers the `Try again!` error, confirming we need to look deeper into the binary to find the correct one.

---

## Step 2 : Decompilation & Logic Analysis

I ran the `strings` command to check for human-readable text hidden inside the binary.

<img width="327" height="201" alt="Image" src="https://github.com/user-attachments/assets/4e7f6d98-a8a7-44c2-bc2e-bea5d1fd4c2c" />

- Found a string `DoYouEven%sCTF`, revealing how the password is structured.
- Spotted `Correct!` and `Try again!`, which helps locate the validation logic later.

I imported the binary into Ghidra and navigated directly to the `main` function to map out the exact logic.

<img width="366" height="418" alt="Image" src="https://github.com/user-attachments/assets/c6406e9d-4b9a-4a1d-8dc8-287fec5be06c" />

- **Input Parsing:** Line 9 shows `__isoc99_scanf("DoYouEven%sCTF", local_28);`. This confirms our string discovery: the program reads user input and automatically expects it to fit into the `DoYouEven<INPUT>CTF` format.
- **Validation Target:** Line 15 shows `strcmp(local_28, "_init")`. The string trapped inside the variable `%s` is directly compared against `_init`.
- **Success Condition:** If the string matches `_init`, the program paths to line 17 and prints `Correct!`.

---

## Step 3 : Finding the Password

By combining the format string found during reconnaissance with the inner logic exposed via Ghidra, the pieces seamlessly fall into place.

- **The Format:** Line 9 shows `scanf("DoYouEven%sCTF", local_28);`. The program automatically wraps whatever text you type inside `DoYouEven` and `CTF`.
- **The Real Check:** Line 15 shows `strcmp(local_28, "_init")`. The program checks if your hidden input inside `%s` is exactly `_init`.
- **Why not other strings?** The binary checks other strings like `__dso_handle` first, but if it hits those, it immediately prints `Try again!` and closes. The only string that paths to `Correct!` is `_init`.

The program constructs the password for you using the format string, so you only need to supply the missing middle piece.

• **The Structure:** `DoYouEven` + `<Your Input>` + `CTF`
• **The Required Value:** `_init`

<img width="630" height="115" alt="Image" src="https://github.com/user-attachments/assets/93238b82-ef23-4802-abf6-6420ecc1b79a" />

**Final Password:**`DoYouEven_init`