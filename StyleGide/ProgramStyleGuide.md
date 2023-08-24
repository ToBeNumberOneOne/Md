# Formatting

## Comments

- inline comments (//) should be preferred to block comments (/* */)  
- Comment delimiters should be separated from the body of the comment by spaces
- Use a commented **line of dashes or equals signs** to separate one section of code

```
// Prefer inline comments

/* Not block comments */

// Make sure there is a space after your delimiter

// If you want to make a section heading, do it like this
//--------------------------------------------------------

// Or like this
//=============================

// You can use your judgement to decide when to wrap
// comments and how long to make your heading lines

```

**Notice**
1. Task comments: Include author and copyright information, a brief description of the task's functionality, version number, and a record of changes and upgrades.  
2. Variable comments: Comments are required at all variable. declarations.
3. Summarizing comments: Introducing a piece of code. Placed at the top of the code.
4. General Comments: Introduces the meaning of the code. Especially critical places need to be annotated

## Variables

**Declarations & Prefixes**

- All variable names should be in lower camel case
- Variable names may contain an informative prefix
- Variables declared in a .var file should only have initial values specified if they are non-zero

```
myLocalVariable		 // variables are lower camel case
or
ActPressure / actPressure

gActPressure        // Global variables start with g

pMyPointer          // Pointers start with p

dMyDynamicVar		// Dynamic variables start with d

diMyDigitalInput    // Digital inputs start with di (similarly, do, ai, ao)
```

**Constant variables**

Use uppercase letters, underline to enhance readability

```
common constant:
HEAT_TIME

step constant:
STEP_INIT
```

**Type declaration**

In Automation Studio, These declarations are within .typ files.  

- **Type declarations should use upper camel case and end in _typ.**
- The name of type cannot start with a number, character.
- The length of variables, data types, functions and functionblocks is limited with 32 characters
```
// General type name
MySubsystemIn_typ

MySubsystemInPar_typ
or
MySubsystemPar_typ
```

**Notice**
Variable declaration table: from top to bottom, invariant constants (e.g., TRUE FALSE), configuration constants (e.g., NB_AXIS), variables.

# Subsystem structures

- Each subsystem or task should be able to function on its own. it should contain the commands required to request actions and give proper responses. most subsystems have stuctures for accepting command/parametters, internally processing, and output statuses

- Members of a structure should be big camel case and desciptive  

- Subsystems should not nest other subsystems in their structures, include axes

Subsystems typically adhere to the follow structure.(full word is also acceptable)

- Subsystem
    - In
        - Cmd
        - Par
        - Cfg
    - Io
        - DiMyInput
        - DoMyOutput
    - Out
        - Done
        - Busy
        - Error
        - ErrorId
        - ErrorString
    - Internal
        - fub *optional*
        - cmd *optional*



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

#### Rule N9 : *Different element types should not bear the same name*
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


---
### Rule CP4: **Driect addressing should not overlap**
---
### Rule CP5: **Applications shall be well designed**
---
### Rule CP6: **Avoid external variables in functions, function blocks and classes**
---
### Rule CP7: **Error information shall be tested**
---
### Rule CP8: **Floating point comparision shall not be equality or inequality**
---
### Rule CP9: **Time and physical measures comparison shall not be equality or inequlity**
---
### Rule CP10: **Limit the comlexity of POU code**
---
### Rule CP11: **Avoid multiple writes from multiple tasks**
---
### Rule CP12: **Manage synchronization among tasks**
---
### Rule CP13: **Physical output should be writen once per PLC cycle**
---
### Rule CP14: **POUs shall not call themselfves directly or indirectly**
---
### Rule CP15: **POUs shall have a single point of exit**
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
