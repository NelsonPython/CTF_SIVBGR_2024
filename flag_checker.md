## Challenge: Flag Checker [Reverse Engineering] 150 points

We found this cryptic Python script that validates the user's flag, but we're having trouble understanding the code. Can you find the correct flag that passes through the program?

[pyrev.py](pyrev.py)

## Solution:

I had two conversations to test whether Microsoft Copilot (public version) could reverse engineer this code.  It generated two different python scripts that both found the flag.  I contacted the leaders of US Cybergames to ask whether this was considered cheating.  They said that using AI was allowed and asked to see this writeup.

### Me:
I literally dumped this block of jumbled code into Copilot:

```
reverse engineer this code phoneSteak = [55, 33, 52, 40, 35, 56, 86, 90, 66, 111, 81, 26, 23, 75, 109, 26, 88, 90, 75, 67, 92, 25, 87, 88, 92, 84, 23, 88] libraryDiscussion = input("Enter the flag: ") confusedSheep = [ord(herdSlot) for herdSlot in libraryDiscussion] mintFarm = len(confusedSheep) trustBreed = len(phoneSteak) seaTent = 6 callCover = 17 foxEmbox = (248 // trustBreed) % trustBreed outfitStrike = 10 brushCopy = (341 // trustBreed) % 17 injectPush = (1240 + 28 // trustBreed) % trustBreed makeupRoof = [] tinRoyalty = [] if trustBreed == mintFarm: for heartCool in confusedSheep: makeupRoof.append(heartCool - 27) for angelStay in makeupRoof: tinRoyalty.append(angelStay ^ 15) franchisePath = tinRoyalty[seaTent] tinRoyalty[seaTent] = tinRoyalty[injectPush] tinRoyalty[injectPush] = franchisePath eastGhostwriter = tinRoyalty[outfitStrike] tinRoyalty[outfitStrike] = tinRoyalty[foxEmbox] tinRoyalty[foxEmbox] = eastGhostwriter personPioneer = tinRoyalty[callCover] tinRoyalty[callCover] = tinRoyalty[brushCopy] tinRoyalty[brushCopy] = personPioneer lineMoon = tinRoyalty[0 : len(tinRoyalty) // 2] puddingCommission = tinRoyalty[len(tinRoyalty) // 2 : len(tinRoyalty)] furRegret = lineMoon + puddingCommission[::-1] tinRoyalty = furRegret if tinRoyalty == phoneSteak: print("Correct!! :)") else: print("Incorrect flag :(") else: print("Incorrect :(")
```

### Copilot:

To reverse engineer this code, we need to understand what it’s doing and then work backwards to find the input that will produce the phoneSteak list when processed by the code.

Here’s a step-by-step breakdown of the code:

    1. libraryDiscussion takes an input string (the flag).

    2. confusedSheep is a list of the ASCII values of the characters in the input string.
    
    3. makeupRoof is created by subtracting 27 from each ASCII value in confusedSheep.
    
    4. tinRoyalty is created by XOR-ing each value in makeupRoof with 15.
    
    5. The code then swaps elements at certain indices in tinRoyalty:
        ◦ seaTent with injectPush
        ◦ outfitStrike with foxEmbox
        ◦ callCover with brushCopy
    
    6. Finally, it reverses the second half of the tinRoyalty list and checks if it matches phoneSteak.

To find the original input, we need to reverse these operations:
    
    1. Reverse the second half of the phoneSteak list.
    
    2. Swap the elements back to their original positions.
    
    3. XOR each number with 15.
    
    4. Add 27 to each number to get the ASCII values.
    
    5. Convert the ASCII values back to characters to get the original string.

Let’s write the reverse code:

copilot.py:
```
phoneSteak = [55, 33, 52, 40, 35, 56, 86, 90, 66, 111, 81, 26, 23, 75, 109, 26, 88, 90, 75, 67, 92, 25, 87, 88, 92, 84, 23, 88]

# Step 1: Reverse the second half of the list
half = len(phoneSteak) // 2
reversedHalf = phoneSteak[half:][::-1]
phoneSteak = phoneSteak[:half] + reversedHalf

# Step 2: Swap the elements back to their original positions
trustBreed = len(phoneSteak)
seaTent = 6
callCover = 17
foxEmbox = (248 // trustBreed) % trustBreed
outfitStrike = 10
brushCopy = (341 // trustBreed) % 17
injectPush = (1240 + 28 // trustBreed) % trustBreed

phoneSteak[seaTent], phoneSteak[injectPush] = phoneSteak[injectPush], phoneSteak[seaTent]
phoneSteak[outfitStrike], phoneSteak[foxEmbox] = phoneSteak[foxEmbox], phoneSteak[outfitStrike]
phoneSteak[callCover], phoneSteak[brushCopy] = phoneSteak[brushCopy], phoneSteak[callCover]

# Step 3 and 4: XOR with 15 and add 27 to each number
originalInput = [chr((num ^ 15) + 27) for num in phoneSteak]

# Step 5: Convert the ASCII values back to characters
originalFlag = ''.join(originalInput)
print(f"The original flag is: {originalFlag}")
```
```                                                                                             
└─$ python3 copilot.py  
The flag is: SIVBGR{pyth0n_r3v3rs1ng_pr0}
```

copilot_2.py:
```
phoneSteak = [55, 33, 52, 40, 35, 56, 86, 90, 66, 111, 81, 26, 23, 75, 109, 26, 88, 90, 75, 67, 92, 25, 87, 88, 92, 84, 23, 88]

# reverse the reversing of the second half
half = len(phoneSteak) // 2
phoneSteak = phoneSteak[:half] + phoneSteak[:half -1:-1]

# reverse the swapping of elements
swap_indices = [(6, (1240 + 28 // 28) % 28), (10, (248 // 28) % 28), (17,(341 //28) % 17)]
for index1, index2 in swap_indices:
    phoneSteak[index1], phoneSteak[index2] = phoneSteak[index2], phoneSteak[index1]

# Reverse the XOR operation and the subtraction
original_values = [((value ^ 15) + 27) for value in phoneSteak]

# Convert ASCII values back to characters
flag = ''.join(chr(value) for value in original_values)
print(f"The flag is : {flag}")
```

```
└─$ python3 copilot_2.py 
The original flag is : SIVBGR{pyth0n_r3v3rs1ng_pr0}
                                                                                             
```

## Experimenting with Google Gemini

### Me:
I dumped the same block of jumbled code into Gemini:

```
reverse engineer this code phoneSteak = [55, 33, 52, 40, 35, 56, 86, 90, 66, 111, 81, 26, 23, 75, 109, 26, 88, 90, 75, 67, 92, 25, 87, 88, 92, 84, 23, 88] libraryDiscussion = input("Enter the flag: ") confusedSheep = [ord(herdSlot) for herdSlot in libraryDiscussion] mintFarm = len(confusedSheep) trustBreed = len(phoneSteak) seaTent = 6 callCover = 17 foxEmbox = (248 // trustBreed) % trustBreed outfitStrike = 10 brushCopy = (341 // trustBreed) % 17 injectPush = (1240 + 28 // trustBreed) % trustBreed makeupRoof = [] tinRoyalty = [] if trustBreed == mintFarm: for heartCool in confusedSheep: makeupRoof.append(heartCool - 27) for angelStay in makeupRoof: tinRoyalty.append(angelStay ^ 15) franchisePath = tinRoyalty[seaTent] tinRoyalty[seaTent] = tinRoyalty[injectPush] tinRoyalty[injectPush] = franchisePath eastGhostwriter = tinRoyalty[outfitStrike] tinRoyalty[outfitStrike] = tinRoyalty[foxEmbox] tinRoyalty[foxEmbox] = eastGhostwriter personPioneer = tinRoyalty[callCover] tinRoyalty[callCover] = tinRoyalty[brushCopy] tinRoyalty[brushCopy] = personPioneer lineMoon = tinRoyalty[0 : len(tinRoyalty) // 2] puddingCommission = tinRoyalty[len(tinRoyalty) // 2 : len(tinRoyalty)] furRegret = lineMoon + puddingCommission[::-1] tinRoyalty = furRegret if tinRoyalty == phoneSteak: print("Correct!! :)") else: print("Incorrect flag :(") else: print("Incorrect :(")
```
Gemini responsed in several increments writing code snippets with spaces for more code.  Then, it asked if I wanted to continue.  This approach reminds me of a professor teaching a student.  But the response was very long with lots of redundant code so I copied the explanation and the final Python script.

### Gemini:

#### Understanding the Code: A Breakdown

The code is designed to verify a user-inputted flag against a predefined set of numbers. If the flag is correct after undergoing a series of transformations, it prints "Correct!!".  Otherwise, it prints an error message.

#### Code Initialization:

- phoneSteak is a list of numbers that represents the correct, encoded flag.
- libraryDiscussion is where the user input (flag) is stored.
- confusedSheep converts the input flag into a list of ASCII values.
- several variables are calculated based on the lengths of phoneSteak and confusedSheep.

#### Flag Processing:

Checks if the lengths of the input and the correct flag match.  If they match:

- makeupRoof subtracts 27 from each element in confusedSheep.
- tinRoyalty performs a XOR operation with 15 on each element in makeupRoof.
- Several elements in tinRoyalty are swapped around using indices calculated earlier.
- tinRoyalty is split in half and reversed, creating furRegret.
- If furRegret matches phoneSteak, the flag is correct.

#### Decoding the Flag

To find the correct flag, we need to reverse the process:

##### Reverse the transformations applied to tinRoyalty:

Undo the splitting and reversing to get back to the original tinRoyalty.

##### Undo the swaps using the same indices.

Undo the XOR operation with 15.

Add 27 to each element.

##### Convert the resulting numbers back to characters:

Convert each number to its corresponding ASCII character.

### Potential Challenges

- The specific values used in calculations (e.g., 27, 15, indices) might have specific meanings or patterns that could aid in analysis.
- The XOR operation can be tricky to reverse without additional information.
- The code uses obscure variable names which might hinder understanding.

### Additional Considerations

- The code could be optimized or made more readable by using more descriptive variable names and potentially combining some steps.
- Error handling could be added to handle cases where the input flag is not the correct length.

Would you like to attempt to reverse engineer the flag based on this information?  I can provide more detailed explanations or hints if needed.

### Me:

yes

### Let's Reverse Engineer the Flag

1. Reverse the final transformation:

Undo the splitting and reversing of tinRoyalty to get back to its original state.

2. Undo the swaps:

Reverse the swaps performed on tinRoyalty using the indices seaTent, injectPush, outfitStrike, foxEmbox, callCover, and brushCopy.

3. Undo the XOR operation:

Perform a XOR operation with 15 on each element of tinRoyalty to reverse the encryption.

4. Undo the subtraction:

Add 27 to each element of tinRoyalty to reverse the offset.

5. Convert to characters:

Convert the resulting numbers to their corresponding ASCII characters to obtain the flag.

6. Recap the swaps:

```
franchisePath = tinRoyalty[seaTent]
tinRoyalty[seaTent] = tinRoyalty[injectPush]
tinRoyalty[injectPush] = franchisePath
eastGhostwriter = tinRoyalty[outfitStrike]
tinRoyalty[outfitStrike] = tinRoyalty[foxEmbox]
tinRoyalty[foxEmbox] = eastGhostwriter
personPioneer = tinRoyalty[callCover]
tinRoyalty[callCover] = tinRoyalty[brushCopy]
tinRoyalty[brushCopy] = personPioneer
```

Essentially, these lines swap elements at specific indices within the tinRoyalty list. To reverse this, we need to perform the same swaps in reverse order.

### Final Python script

```
def reverse_engineer(phoneSteak):
  # Reverse the final transformation
  tinRoyalty = phoneSteak[:len(phoneSteak) // 2] + phoneSteak[len(phoneSteak) // 2:][::-1]

  # Undo the swaps
  seaTent = 6
  injectPush = 10
  outfitStrike = 10
  foxEmbox = 12
  callCover = 17
  brushCopy = 17

  # Reverse the swaps
  personPioneer = tinRoyalty[brushCopy]
  tinRoyalty[brushCopy] = tinRoyalty[callCover]
  tinRoyalty[callCover] = personPioneer

  eastGhostwriter = tinRoyalty[foxEmbox]
  tinRoyalty[foxEmbox] = tinRoyalty[outfitStrike]
  tinRoyalty[outfitStrike] = eastGhostwriter

  franchisePath = tinRoyalty[injectPush]
  tinRoyalty[injectPush] = tinRoyalty[seaTent]
  tinRoyalty[seaTent] = franchisePath

  # Undo the XOR operation
  tinRoyalty = [x ^ 15 for x in tinRoyalty]

  # Undo the subtraction
  tinRoyalty = [x + 27 for x in tinRoyalty]

  # Convert to characters
  flag = ''.join(chr(x) for x in tinRoyalty)
  return flag

# Replace with the actual phoneSteak values
phoneSteak = [55, 33, 52, 40, 35, 56, 86, 90, 66, 111, 81, 26, 23, 75, 109, 26, 88, 90, 75, 67, 92, 25, 87, 88, 92, 84, 23, 88]

flag = reverse_engineer(phoneSteak)
print(flag)
```

### Output:
Gemini ran the code online and output the following flag:

FLAG{REVERSE_ENGINEERING_IS_FUN}

This appears to be a hallucination that does not match the original flag or the flag output from the Python script that Gemini coded.  When I ran the Gemini code, I got this flag:

The Gemini flag is: SIVBGR3ph{t0y_r3vnrs1ng_pr0}

It is close to the original flag, but Gemini computed a "3ph{t0y" instead of a "{pyth0n" and the "n" after "v" should be a "3".  Otherwise the flag matches the original:

The original flag is : SIVBGR{pyth0n_r3v3rs1ng_pr0}




