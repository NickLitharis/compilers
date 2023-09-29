# Date Format Analyzer (DFA)

This project implements a Date Format Analyzer (DFA) to validate and extract dates from text input based on specific date formats. It utilizes the DFA module from the `compilerlabs` library, allowing you to define and analyze date formats with ease.

## About the Author
- **Name:** Νίκος Λιθαρής
- **ΑΜ (Student ID):** Π2019083

## Introduction
The Date Format Analyzer (DFA) is a tool for parsing and validating dates within text using predefined date formats. It leverages the DFA module from the `compilerlabs` library, which simplifies the process of creating and using deterministic finite automata (DFA) for various purposes, including date recognition.

## Usage
To use the Date Format Analyzer, you need to follow these steps:

1. Create a DFA instance for date recognition:

   ```python
   lab1dfa = DFA('s0')
   ```

2. Define the DFA transitions to recognize date patterns. The transitions are defined based on your expected date formats. In this example, we validate date formats in the form of "dd/mm/yyyy," "dd-mm-yyyy," and "dd.mm.yyyy":

   ```python
   # Date Format: dd/mm/yyyy, dd-mm-yyyy, dd.mm.yyyy
   lab1dfa.transition('s0', '0', 's1')
   lab1dfa.transition('s1', ['1', '2', '3', '4', '5', '6', '7', '8', '9'], 's2')
   lab1dfa.transition('s2', ['/', '-', '.'], 's3')

   # Date Format: dd/yyyy
   lab1dfa.transition('s0', ['1', '2'], 's4')
   lab1dfa.transition('s4', ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'], 's2')
   lab1dfa.transition('s4', ['/', '-', '.'], 's3')

   # Date Format: d/m/yyyy, d/mm/yyyy, dd/m/yyyy
   lab1dfa.transition('s0', '3', 's5')
   lab1dfa.transition('s5', ['0', '1'], 's2')
   lab1dfa.transition('s5', ['/', '-', '.'], 's3')

   # Date Format: mm/yyyy, m/yyyy
   lab1dfa.transition('s3', '0', 's6')
   lab1dfa.transition('s6', ['1', '2', '3', '4', '5', '6', '7', '8', '9'], 's7')
   lab1dfa.transition('s7', ['/', '-', '.'], 's8')

   # Date Format: yyyy
   lab1dfa.transition('s8', '1', 's10')
   lab1dfa.transition('s10', '9', 's11')
   lab1dfa.transition('s11', ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'], 's12')
   lab1dfa.transition('s12', ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'], 's13')
   lab1dfa.transition('s8', '2', 's14')
   lab1dfa.transition('s14', '0', 's11')
   ```

3. Define the accepted states. In this case, we accept the state 's13' as a valid date:

   ```python
   lab1dfa.accept('s13', 'DATE ACCEPTED')
   ```

4. Use the `scan` method to scan text for date patterns and extract valid dates:

   ```python
   token, lexeme = lab1dfa.scan("31/3/2023")
   print(token, lexeme)  # Output: DATE ACCEPTED 31/3/2023
   ```

   You can use the `token` and `lexeme` to identify and process valid date occurrences in your text.

## Example Tests
Here are some test cases to demonstrate the DFA's behavior:

- `"31/3/2023"` will be recognized as `DATE ACCEPTED 31/3/2023`.
- `"7-02-1901"` will not be recognized as a valid date.
- `"09.11.2088"` will be recognized as `DATE ACCEPTED 09.11.2088`.
- `"32/8/1980"` will not be recognized as a valid date.
- `"05-13-2023"` will not be recognized as a valid date.
- `"4.6.1899"` will not be recognized as a valid


# Web Page Text Parser

This Python script is designed to parse and extract information from a web page. It performs various tasks to clean and process the HTML content retrieved from a web page. Let's break down the steps of this script:

## Step 1: Read Web Page Text
The script starts by reading the text content of a web page located at the URL "http://mixstef.github.io/courses/compilers/testpage.txt." It uses the `urllib.request` module to fetch the page content.

## Step 2: Extract and Print the Title
The script uses regular expressions to extract and print the title of the web page. It searches for text enclosed within `<title>` and `</title>` tags and prints the title if found.

## Step 3: Delete Comments
This step involves the removal of HTML comments from the web page content. It uses a regular expression to match and remove comment blocks (`<!-- ... -->`).

## Step 4: Remove Script and Style Tags
The script removes `<script>` and `<style>` tags along with their content from the web page content. It ensures that JavaScript and CSS code do not interfere with further processing.

## Step 5: Extract and Print Links and Link Text
In this step, the script identifies and prints both the links and their corresponding link text. It uses a regular expression to locate `<a>` tags, extract the `href` attribute (link), and the text between the opening and closing `<a>` tags.

## Step 6: Delete All Tags
All remaining HTML tags are removed from the web page content. This step ensures that only the textual content remains.

## Step 7: Convert HTML Entities
HTML entities (e.g., `&amp;`, `&gt;`, `&lt;`, `&nbsp;`) are converted back to their respective characters (`&`, `>`, `<`, space) to ensure proper text representation.

## Step 8: Convert Consecutive Whitespace
Sequences of consecutive whitespace characters are reduced to a single space character. This step helps improve the readability of the processed text.

## Step 9: Print the Final Formatted Text
The script prints the final formatted text, which is now free of HTML tags, comments, and HTML entities.

Overall, this script provides a basic example of web page text parsing and demonstrates the use of regular expressions for HTML content manipulation. You can further customize and extend it to suit your specific web scraping needs.


# Simple Arithmetic Expression Evaluator with Lexer, Parser, and Interpreter

## Lexer
The lexer is implemented using the `plex` library and defines regular expressions for tokens such as numbers, operators, keywords (in this case, just "print"), identifiers (variable names), and whitespace.

## Parser
The parser is implemented as a recursive descent parser. It defines parsing rules for arithmetic expressions, statements, and a statement list. It builds an abstract syntax tree (AST) while parsing.

- `stmt_list`: Parses a list of statements.
- `stmt`: Parses individual statements, which can be either assignments or print statements.
- `expr`: Parses arithmetic expressions, taking into account operator precedence.
- `term`: Parses terms in expressions, like multiplication and division.
- `factor`: Parses factors, including numbers, variables, and sub-expressions within parentheses.
- `addop` and `multop`: Parse addition/subtraction and multiplication/division/modulo/exponentiation operators, respectively.

## Interpreter
The interpreter is responsible for executing the AST generated by the parser. It supports assignment (`=`) and print statements (`print`). The `compute` function recursively evaluates arithmetic expressions. Variables are stored in a symbol table (`self.symbol_table`).

## Main Part
In the main part of the script:
1. A sample input text with arithmetic expressions and print statements is defined.
2. A scanner is created using the defined lexer.
3. The parser is used to parse the input text, generating an AST.
4. The interpreter is used to run the AST, computing and printing the results.

Overall, this script can parse and execute simple arithmetic expressions and print statements. It demonstrates the use of lexing, parsing, and interpreting techniques for a basic programming language subset.

If you have any specific questions or need further assistance with this code, please let me know.
