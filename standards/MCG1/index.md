### Morning Programming Language Documentation (MCG-1 Standard)  
**Version 1.0** | © 2025 Morning Language Foundation  

---

#### 1. Lexical Structure  
**1.1 S-Expressions**  
*Syntax:* `[operator operand1 operand2 ...]`  
*Purpose:* Fundamental syntactic unit for code and data representation. Enables homoiconicity.  
*Example:* `[+ 5 (* 3 2)]` → 11  

**1.2 Comments**  
*Syntax:* `// Single-line`, `/* Multi-line */`  
*Purpose:* Code documentation and exclusion of expressions.  

---

#### 2. Primitive Types  
**2.1 !int**  
*Size:* 64-bit (default integer)  
*Purpose:* Integer arithmetic. Supports decimal, hex (0x), octal (0o), binary (0b).  
*Example:* `[var (x !int) 0b1010]`  

**2.2 !frac**  
*Size:* 64-bit floating point  
*Purpose:* Fractional arithmetic.  
*Example:* `[var (pi !frac) 3.14159]`  

**2.3 !bool**  
*Size:* 1 byte  
*Values:* `true`, `false`  
*Purpose:* Boolean logic.  
*Example:* `[var (flag !bool) true]`  

**2.4 !str**  
*Structure:* Pointer to null-terminated UTF-8 string  
*Purpose:* Text manipulation.  
*Example:* `[var (name !str) "Alice"]`  

**2.5 !ptr**  
*Structure:* Generic memory address  
*Purpose:* Raw memory access.  
*Example:* `[var (addr !ptr) (mem-alloc 16)]`  

**2.6 !none**  
*Purpose:* Void type for functions without return.  
*Example:* `[func noop () -> !none []]`  

---

#### 3. Composite Types  
**3.1 !array<T,N>**  
*Structure:* Fixed-size contiguous memory block  
*Purpose:* Homogeneous collections.  
*Example:* `[var (nums !array<!int,3>) (array 1 2 3)]`  

**3.2 !size:N!T**  
*Constraint:* Size-constrained type  
*Purpose:* Memory layout control.  
*Example:* `[var (packed !size:4!int) 255]`  

---

#### 4. Variable Declarations  
**4.1 var**  
*Syntax:* `[var (name type) value]`  
*Purpose:* Mutable variable binding.  
*Example:* `[var (counter !int) 0]`  

**4.2 const**  
*Syntax:* `[const (name type) value]`  
*Purpose:* Immutable binding.  
*Example:* `[const (MAX_SIZE !int) 100]`  

---

#### 5. Control Flow  
**5.1 if/elif/else**  
*Syntax:*  
```morning
[if
  condition1  block1
  elif condition2 block2
  else block3
]
```  
*Purpose:* Conditional execution.  

**5.2 while**  
*Syntax:* `[while condition body]`  
*Purpose:* Pre-tested loop.  
*Example:*  
```morning
[while [> x 0]
  [set x [- x 1]]
]
```

**5.3 for**  
*Syntax:* `[for init condition step body]`  
*Purpose:* C-style loop.  
*Example:*  
```morning
[for [var i 0] [< i 10] [set i [+ i 1]]
  [fprint "%d " i]
]
```

**5.4 loop**  
*Syntax:* `[loop body]`  
*Purpose:* Infinite loop with explicit `break`.  
*Example:*  
```morning
[loop
  [set x [- x 1]]
  [check [== x 0] [break]]
]
```

**5.5 break**  
*Purpose:* Exit innermost loop.  

**5.6 continue**  
*Purpose:* Skip to next loop iteration.  

**5.7 check**  
*Syntax:* `[check condition then-expr else-expr]`  
*Purpose:* Ternary conditional.  
*Example:* `[check [> x 0] 1 -1]`  

---

#### 6. Functions  
**6.1 func**  
*Syntax:*  
```morning
[func name ((param1 type1) ...) -> return_type
  body
]
```  
*Purpose:* Function definition.  
*Example:*  
```morning
[func square ((x !int)) -> !int
  [* x x]
]
```

**6.2 Function Call**  
*Syntax:* `[name arg1 arg2 ...]`  
*Example:* `[square 5]` → 25  

---

#### 7. Memory Operations  
**7.1 mem-alloc**  
*Syntax:* `[mem-alloc size]`  
*Returns:* `!ptr`  
*Purpose:* Dynamic memory allocation.  
*Example:* `[var buf [mem-alloc 256]]`  

**7.2 mem-free**  
*Syntax:* `[mem-free ptr]`  
*Purpose:* Deallocate memory.  

**7.3 mem-read**  
*Syntax:* `[mem-read ptr type]`  
*Purpose:* Read typed value from memory.  
*Example:* `[mem-read buf !int]`  

**7.4 mem-write**  
*Syntax:* `[mem-write ptr value]`  
*Purpose:* Write value to memory address.  

**7.5 mem-ptr**  
*Syntax:* `[mem-ptr variable]`  
*Returns:* `!ptr`  
*Purpose:* Get address of variable.  

**7.6 mem-deref**  
*Syntax:* `[mem-deref ptr type]`  
*Purpose:* Dereference typed pointer.  

**7.7 byte-read/byte-write**  
*Purpose:* Low-level byte access.  
*Example:* `[byte-write buf 0x41]`  

---

#### 8. I/O Operations  
**8.1 fprint**  
*Syntax:* `[fprint format arg1 ...]`  
*Purpose:* Formatted output (C-style).  
*Example:* `[fprint "Value: %d" x]`  

**8.2 finput**  
*Syntax:* `[finput format var1 ...]`  
*Purpose:* Formatted input.  
*Example:* `[finput "%d" age]`  

---

#### 9. Bitwise Operations  
**9.1 bit-and/or/xor**  
*Syntax:* `[bit-and a b]`  
*Purpose:* Bit-level manipulation.  

**9.2 bit-shl/shr**  
*Purpose:* Bit shifting.  

**9.3 bit-not**  
*Purpose:* Bit inversion.  

---

#### 10. Special Forms  
**10.1 scope**  
*Syntax:* `[scope expr1 expr2 ...]`  
*Purpose:* Lexical scoping block.  

**10.2 set**  
*Syntax:* `[set variable value]`  
*Purpose:* Variable mutation.  

**10.3 index**  
*Syntax:* `[index array position]`  
*Purpose:* Array element access.  
*Example:* `[index nums 0]`  

---

#### 11. Type Operations  
**11.1 sizeof**  
*Syntax:* `[sizeof type]`  
*Returns:* Size in bytes.  
*Example:* `[sizeof !int]` → 8  

---

**The Morning Manifesto**:  
*"We reject the false choice between performance and expressiveness.  
We reject the old methods imposed by backward compatibility with  
long-dead legacy products. Morning is a new era in programming  
that combines the simplicity of S-expressions with the functionality  
of C++. Thanks to the purity of the project and its versatility,  
you can create anything and everything you want."*  

---  
*MCG-1 Standard © 2025 Morning Language Foundation*  
*LLVM® is a registered trademark of the LLVM Project*
