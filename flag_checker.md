## Challenge: Flag Checker [Reverse Engineering] 150 points

We found this cryptic Python script that validates the user's flag, but we're having trouble understanding the code. Can you find the correct flag that passes through the program?

[pyrev.py](pyrev.py)

## Solution:

I had two conversations to test whether Microsoft Copilot (public version) could reverse engineer this code.  It generated two different python scripts that both found the flag.  I contacted the leaders of US Cybergames to ask whether this was considered cheating.  They said that using AI was allowed and asked to see this writeup.

### Me:
reverse engineer this code phoneSteak = [55, 33, 52, 40, 35, 56, 86, 90, 66, 111, 81, 26, 23, 75, 109, 26, 88, 90, 75, 67, 92, 25, 87, 88, 92, 84, 23, 88] libraryDiscussion = input("Enter the flag: ") confusedSheep = [ord(herdSlot) for herdSlot in libraryDiscussion] mintFarm = len(confusedSheep) trustBreed = len(phoneSteak) seaTent = 6 callCover = 17 foxEmbox = (248 // trustBreed) % trustBreed outfitStrike = 10 brushCopy = (341 // trustBreed) % 17 injectPush = (1240 + 28 // trustBreed) % trustBreed makeupRoof = [] tinRoyalty = [] if trustBreed == mintFarm: for heartCool in confusedSheep: makeupRoof.append(heartCool - 27) for angelStay in makeupRoof: tinRoyalty.append(angelStay ^ 15) franchisePath = tinRoyalty[seaTent] tinRoyalty[seaTent] = tinRoyalty[injectPush] tinRoyalty[injectPush] = franchisePath eastGhostwriter = tinRoyalty[outfitStrike] tinRoyalty[outfitStrike] = tinRoyalty[foxEmbox] tinRoyalty[foxEmbox] = eastGhostwriter personPioneer = tinRoyalty[callCover] tinRoyalty[callCover] = tinRoyalty[brushCopy] tinRoyalty[brushCopy] = personPioneer lineMoon = tinRoyalty[0 : len(tinRoyalty) // 2] puddingCommission = tinRoyalty[len(tinRoyalty) // 2 : len(tinRoyalty)] furRegret = lineMoon + puddingCommission[::-1] tinRoyalty = furRegret if tinRoyalty == phoneSteak: print("Correct!! :)") else: print("Incorrect flag :(") else: print("Incorrect :(")

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

Python
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

Both scripts will find the flag: 
SIVBGR{pyth0n_r3v3rs1ng_pr0}

