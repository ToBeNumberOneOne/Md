# Coding Guidelines 

**from PLCopen Promotional Committee Training** [Download](https://www.plcopen.org/system/files/downloads/plcopen_coding_guidelines_version_1.0.pdf)

## Nameing Rules

### Rule N1 : **Avoid physical addresses**

- **Guideline**: Using physical addresses in programs is forbidden - always define a Variable.  

- **Exceptions**: In some communication protocols a physical addressing is needed to assign variables to a physical location for the order in the communication structure.

---
### Rule N2 : **Define type prefixes for Variables**
---
### Rule N3 : **Define the names to avoid**
    
- **Guideline**:
    - IEC data types and standard library object names must be avoided
    - IEC Structured Text keywords must be avoided
    - Reserved words (for your platform) must be avoided
    - Avoid uncommon abbreviations (or specify them clearly)
    - Avoid meaningless names like: Info, Data, Temp, Str, Buf.

---
### Rule N4 : **Define the use of case (capitals)**

- **Guideline**: Use the same capitalization for every object instance, even if the tool/compiler doesn't mandate it.
The following guidelines are proposed:
    - Use UPPER_SNAKE_CASE for **CONSTANTS** and user defined datatypes and keywords (like BOOL, FOR, TYPE and END_TYPE).
    - Use UpperCamelCase for all other multi-word items
    
- **Exceptions**: When using **prefixes** for Variables (see Rule 14) the UpperCamelCase changes to lowerCamelCase.

---
### Rule N5 : **Local names shall not shadow global names**

- **Guideline**: Do not use identical names for any tasks programs, functions and function blocks, variables, user defined types, and name spaces. **Using prefixes** to name your programming elements may help (see rule N2 3.1.2 Define type prefixes for Variables (if used)).

---
### Rule N6 : *Define an acceptable name length*
    
- **Guideline**: 
    - Min. length proposal: 8 characters, or 3 characters for local names.
    - Max. length proposal: 25 characters
    - On the average: keep the maximum to 15 characters
    - Don't use abbreviations, unless required to shorten the name and the abbreviations are expected to be well known.
    - Avoid names that are very similar or different only in case. For example, avoid using the names like variable and variables.

- **Exceptions**: It is not required for some elements of limited scope (e.g. Internal Variables):
    - loop indexes: as their usage is limited to a given loop and it is common usage to use very short name
    - structure members: the structure member name is always used with the instance name so it may be meaningful even when being short.

---

### Rule N7 : *Define naming rules for namespaces*

---

### Rule N8 : *Define the acceptable character set*

- Identifiers should use characters from ASCII 7 bits.

---

### Rule N9 : *Different element types should not bear the same name*
- make code difficult to read even if compilation is possible

- **Guideline**: Do not use identical names for any tasks programs, functions and function blocks, variables, UDTs and name spaces.
   
---

### Rule N10 : *Define name prefixes for user defined types*

---

## Comment Rules

### Rule C1 : **Comments shall describe the intention of the code**

- **Guideline**: Review code to ensure it is explained by a comment, either:
    - At the end of the code line
    - Separate comment, immediately prior to the code
    - Block comment and the start of a sub section of code (e.g. whole IF THEN ELSE block)
    - Function/Function Block comment, if the code is small enough to be wholly explained by the comment
    - Program comment, if the code is small enough to be wholly explained by the comment

- **Exceptions**: Any use of **'magic numbers' or physical addresses** should always be commented

---

### Rule C2 : **All elements shall be commented**

- **Guideline**: Include clear comments for all elements, including Tasks, Programs, Function Blocks, Functions, User Defined Types, Variables and Resources.
---

### Rule C3 : *Avoid nested comments*

- **Guideline**: In the released versions of programs there should be no nested comments, even if it's possible during development and debug phases.

---

### Rule C4 : *Comments may not include code*

- **Guideline**: In the released versions of programs there should be no nested comments, even if it's possible during development and debug phases.
---

### Rule C5 : *Use single line comments*

- **Exceptions**: During testing and debugging phases, multi-line comments can be used to exclude portion of codes. Single line comments with // should not be used for this purpose.

---

### Rule C6 : *Define comments language* :

- Use English as the comments languase

---

## Coding Practice

### Rule CP1: **Access to a member shall be by name**

- **Guideline**: Access to a member by offset should be avoided.

---

### Rule CP2: **All code should be used in the application**

- **Guideline**: All parts of the application code should be reachable under certain conditions so that they are not `dead code`.

Different kinds of dead code exists:
- Unreferenced functions : 
    - re-use of an already existing application to create a new application slightly different
    - developer mistake forgetting to call part of the application during development
    - part of the code used in a specific environment : testing, simulation of missing hardware
- Code not reachable unconditionally - techniques used by developer to bypass part of the code :
    - due to unconditional go to followed by code without called label
    - condition always false or true

- **Exceptions**: A dead code with a nice comment delimiting and explaining correctly the reason why 
the code was bypassed may be used. In such case, the code section should be small enough so that a developer sees as evidence that it is dead code.

---
### Rule CP3: **All Variables should be initialized before being used**

- **Exceptions**:
    - If the PLC initializes explicitly variables to 0, and if this variable should be initialized to 0, it is not required to explicitly initialize the variables to zero. However this can affect portability of the code.
    - Variables that have RETAIN attribute will automatically have their values initialized to their retained upon power reset.
    - Variables that are linked to physical inputs do not need to be initialized

---
### Rule CP4: **Driect addressing should not overlap**
---
### Rule CP5: **Applications shall be well designed**

Related: Rule CP15 and Rule CP26

- **Guideline**:
    - Applications must be well designed
    - Make use of Arrays (where supported) to aggregate related variables of the same data type
    - Make use of Structures (where supported) to aggregate related variables of different data types
    - Make use of Classes where supported or Functions and Function Blocks to reduce complexity of large areas into smaller parts
    - Make use of Classes where supported, or Functions and Function Blocks to reuse common code
    - Make use of PRIVATE variables in Classes where supported, or Function Block internal variables to encapsulate data
    - Design separate programs to use the best language for the job: Ladder, Structured Text, Sequential Function Chart or Function Block Diagram (where supported)

- **Exceptions**: Accessing system-defined variables or VAR_GLOBAL CONSTANT may require the use of external references

---
### Rule CP6: **Avoid external variables in functions, function blocks and classes**

- **Guideline**:
    - Functions, Function Blocks and Classes **should not use external variables**.
    - A good alternative to using external references to global variables can be to extend the parameter list, and pass the variables needed for access.
---
### Rule CP7: **Error information shall be tested**

- **Guideline**:  After a call to a function returning error information, the returned error information should be tested and eventually the behavior of the process should be changed in case of error.

---
### Rule CP8: **Floating point comparision shall not be equality or inequality**

- **Guideline**: comparison between floating point variables must use only the following operators: strict less than (<), less than or equal (<=), strict greater than (>), greater than or equal (>=).

- Many numbers cannot be represented exactly in floating point notation: number 0.1 is represented by a binary value that corresponds to decimal 0.100000001490116119384765625 in 24bits single precision
---
### Rule CP9: **Time and physical measures comparison shall not be equality or inequlity**

- **Guideline**: Comparison between time information or physical measures must use only the following operators: strict less than (<), less than or equal (<=), strict greater than (>), greater than or equal (>=).

- **Reasoning**: The equality operator requires a strict equality between operands. When using time information or physical measure, this equality may not be met and the inequality is almost alwaysmet.
---
### Rule CP10: **Limit the comlexity of POU code**

- **Guideline**: If the code of a POU exceeds some complexity level, the code should be split up into several POUs.

- **Reasoning**: Complex code is difficult to maintain and a source of errors.
---
### Rule CP11: **Avoid multiple writes from multiple tasks**

- **Guideline**: When sharing variables among tasks you must avoid writing to a variable from more than one task. When using communication variables between two tasks, it is important to decide which task writes which variables so that a given variable is not written from the two different tasks.

---
### Rule CP12: **Manage synchronization among tasks**
---
### Rule CP13: **Physical output should be writen once per PLC cycle**
---
### Rule CP14: **POUs shall not call themselfves directly or indirectly**

- **Guideline**: A recursive algorithm should be replaced by an iterative algorithm.

- **Reasoning**: Recursive algorithms normally consume stack space for each call so deep recursion can cause system failure, even if unplanned. Support for recursive calls of POU is vender-specific so using it also makes the program less portable

---
### Rule CP15: **POUs shall have a single point of exit**

- **Guideline**: POU structure should be changed to avoid the usage of RETURN instruction before the end of the code. If programming language is Structured Text, conditional instructions should be used for the other languages, use a label at the end of the POU and use JUMP instruction to jump to this last label.

- **Reasoning**: Testability, Readability and maintainability are good reasons to do that. In case of debugging, it is possible to add some code just before returning the POU and we got this information in all cases.

---
### Rule CP16: **Read a variable writtten by anoyher task only once per cycle**
---
### Rule CP17: **Taks shall only call program POUs and not function block**
---
### Rule CP18: **Usage of parameters shall match their declaration mode**
---
### Rule CP19: **Use of gloable variables shall be limited**
---
### Rule CP20: **Usage of Jump and Return should be a voided**
---
### Rule CP21: **Function block instance should only call once**
---
### Rule CP22: **Use VAR_TEMP for temporary variable declaration**
---
### Rule CP23: **Select Appropriate Data Type**
---
### Rule CP24: **Define maximum number of input/output/in-out variables of a POU**
---
### Rule CP25: **Do not declare variables that are not used**
---
### Rule CP26: **Data types conversion should be explicit**
---
### Rule CP27: **A global variable may be written only by one PROGRAM**
---
### Rule CP28: **Avoid deprecated features**
---

## Program Language

## Vendor Specific IEC61131-3 Extensions
