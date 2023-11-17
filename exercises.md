# Assignment 9

## 10.1

### 10.1.i

* ADD:
    `s[sp - 1] = Tag(Untag(s[sp - 1]) + Untag(s[sp])); sp--; break;`
    untag the value one below the stack pointer, and untag the value at the stack pointer.
    Then add them together, and store the result one below the stack pointer, with a tag.
    Reduce the stack pointer by one, and finish.
* CSTI: 
    `s[sp + 1] = Tag(p[pc++]); sp++; break;`
    gets the argument to CSTI, and increases PC, tags the value, places it one above the stack pointer, increments the stack pointer, and finishes.
* NIL:
    `s[sp + 1] = 0; sp++; break;`
    The difference between NIL and CSTI 0, is the tag. Nil is not tagged, and is therefore a reference, where as CSTI 0, is tagged as a int.
* IFZERO:
    `word v = s[sp--]; pc = (IsInt(v) ? Untag(v) == 0 : v == 0) ? p[pc] : pc + 1;`
    It determines if the element on the top of the stack is a reference or int.
    If it is an int it untags it, and compares it to zero. Otherwise it just compares it to 0.
    If the expression is true, the program counter is changed to the argument to IFZERO.
* CONS:

    ```{}
    {
      word* p = allocate(CONSTAG, 2, s, sp);
      p[1] = (word)s[sp - 1];
      p[2] = (word)s[sp];
      s[sp - 1] = (word)p;
      sp--;
    } break;`
    ```

    It allocates space for two CONSTAG elements on the heap.
    Then it extracts the top two elements from the stack, and assigns them to the new pair.
    Finally it places the reference to the pair, one below the top of the stack, and decrees the stack pointer by one
* CAR:

    ```{}
    {
        word* p = (word*)s[sp];
        if (p == 0) {
            printf("Cannot take car of null\n"); return -1;
        }
        s[sp] = (word)(p[1]);
    } break;
    ```

    Loads the first element of a heap allocated pair, created with the CONS word.
    It does this, by getting the value of the reference from the stack.
    Then it performs a NIL check.
    If the check succeeds, then the top of the stack is replaced by the first element of the CONS pair.
* SETCAR:

    ```{}
    {
      word v = (word)s[sp--];
      word* p = (word*)s[sp];
      p[1] = v;
    } break;
    ```

    Sets the head of a CONS pair given by a reference, to the value on top of the stack stack.
    It does this by getting the new value and reference from the stack, and assigning the value to the CONS pair.

### 10.1.ii

The Length macro determines the number of blocks associated with the header.

The Colour macro gives the colour of the block header.

The Paint macro sets the colour of the block header.

### 10.1.iii

allocate() is only called when interpreting the CONS instructions, since the CONS is the only heap allocated data structure in the abstract machine.

there are no other interactions between the garbage collector and the abstract machine.

### 10.1.iv

allocate() is the only function that calls collect(), and only in the case that there are no more heap space available to allocate.

## 10.2

We have made changes to markPhase and sweepPhase in listmachine.c.
We have also added the function mark.

Below are two outputs from running and testing, ex35.out, and ex36.out.

```{}
ListVM>listmachine.exe ex35.out
33 33 
Heap: 333 blocks (666 words); of which 0 free (0 words, largest 0 words); 1 orphans
Heap: 333 blocks (666 words); of which 0 free (0 words, largest 0 words); 1 orphans
Heap: 333 blocks (666 words); of which 330 free (660 words, largest 2 words); 1 orphans
44 44
Used 0 cpu milli-seconds
```

```{}
ListVM>listmachine.exe ex36.out
1 Heap: 333 blocks (666 words); of which 0 free (0 words, largest 0 words); 1 orphans
Heap: 333 blocks (666 words); of which 0 free (0 words, largest 0 words); 1 orphans
Heap: 333 blocks (666 words); of which 331 free (662 words, largest 2 words); 1 orphans
1
Used 0 cpu milli-seconds
```

## 10.3

We have upgraded the sweepPhase function, to make new larger blocks.

Below are two outputs from running and testing, ex35.out, and ex36.out, with the new garbage collector.

```{}
ListVM>listmachine.exe ex35.out
33 33 Heap: 333 blocks (666 words); of which 0 free (0 words, largest 0 words); 1 orphans
Heap: 333 blocks (666 words); of which 0 free (0 words, largest 0 words); 1 orphans
Heap: 168 blocks (831 words); of which 165 free (825 words, largest 5 words); 1 orphans
44 44
Used 0 cpu milli-seconds
```

```{}
ListVM>listmachine.exe ex36.out
1 Heap: 333 blocks (666 words); of which 0 free (0 words, largest 0 words); 1 orphans
Heap: 333 blocks (666 words); of which 0 free (0 words, largest 0 words); 1 orphans
Heap: 168 blocks (831 words); of which 166 free (827 words, largest 5 words); 1 orphans
1
Used 0 cpu milli-seconds
```
