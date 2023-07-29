### lexer Interface class

```kotlin
class Token(val type: TokenType, val value: String)

enum class TokenType {
    // Single-character tokens
    LEFT_PAREN, RIGHT_PAREN, LEFT_BRACE, RIGHT_BRACE,
    COMMA, DOT, SEMICOLON, PLUS, MINUS, STAR, SLASH,

    // One or two character tokens
    BANG, BANG_EQUAL, EQUAL, EQUAL_EQUAL,
    GREATER, GREATER_EQUAL, LESS, LESS_EQUAL,

    // Literals
    IDENTIFIER, STRING, NUMBER,

    // Keywords
    INGREDIENT, FREEZE, RECIPE, PLATE, TASTE, ELSESPICE, SWALLOW,
    STIR, BAKE, GIVEUP, NEXTPLS, FAVORITE, CASE, OTHERWISE,
    SERVE, ORDER, MENU, FLAVOR, TRUE, FALSE,

    // Special tokens
    EOF
}
```
### Responsibility

- *Input* : String / File loading

- **Jobs** : 1 job converting strings to List of tokens


### Example implementation

```kotlin
class Token(val type: TokenType, val value: String)

enum class TokenType {
    // Single-character tokens
    LEFT_PAREN, RIGHT_PAREN, LEFT_BRACE, RIGHT_BRACE,
    COMMA, DOT, SEMICOLON, PLUS, MINUS, STAR, SLASH,

    // One or two character tokens
    BANG, BANG_EQUAL, EQUAL, EQUAL_EQUAL,
    GREATER, GREATER_EQUAL, LESS, LESS_EQUAL,

    // Literals
    IDENTIFIER, STRING, NUMBER,

    // Keywords
    INGREDIENT, FREEZE, RECIPE, PLATE, TASTE, ELSESPICE, SWALLOW,
    STIR, BAKE, GIVEUP, NEXTPLS, FAVORITE, CASE, OTHERWISE,
    SERVE, ORDER, MENU, FLAVOR, TRUE, FALSE,

    // Special tokens
    EOF
}

class Lexer(private val source: String) {
    private val tokens = mutableListOf<Token>()
    private var start = 0
    private var current = 0
    private var line = 1

    fun tokenize(): List<Token> {
        while (!isAtEnd()) {
            start = current
            scanToken()
        }

        tokens.add(Token(TokenType.EOF, ""))
        return tokens
    }

    private fun scanToken() {
        val c = advance()
        when (c) {
            '(' -> addToken(TokenType.LEFT_PAREN)
            ')' -> addToken(TokenType.RIGHT_PAREN)
            '{' -> addToken(TokenType.LEFT_BRACE)
            '}' -> addToken(TokenType.RIGHT_BRACE)
            ',' -> addToken(TokenType.COMMA)
            '.' -> addToken(TokenType.DOT)
            ';' -> addToken(TokenType.SEMICOLON)
            '+' -> addToken(TokenType.PLUS)
            '-' -> addToken(TokenType.MINUS)
            '*' -> addToken(TokenType.STAR)
            '/' -> addToken(TokenType.SLASH)
            '!' -> addToken(if (match('=')) TokenType.BANG_EQUAL else TokenType.BANG)
            '=' -> addToken(if (match('=')) TokenType.EQUAL_EQUAL else TokenType.EQUAL)
            '<' -> addToken(if (match('=')) TokenType.LESS_EQUAL else TokenType.LESS)
            '>' -> addToken(if (match('=')) TokenType.GREATER_EQUAL else TokenType.GREATER)
            '"' -> stringLiteral()
            in '0'..'9' -> numberLiteral()
            in 'a'..'z', in 'A'..'Z', '_' -> identifier()
            ' ', '\r', '\t' -> { /* Ignore whitespace */ }
            '\n' -> line++
            else -> println("Error: Unexpected character '$c'")
        }
    }

    private fun identifier() {
        while (peek().isLetterOrDigit() || peek() == '_') {
            advance()
        }
        val text = source.substring(start, current)
        addToken(keywords.getOrDefault(text, TokenType.IDENTIFIER))
    }

    private fun numberLiteral() {
        while (peek().isDigit()) {
            advance()
        }
        if (peek() == '.' && peekNext().isDigit()) {
            advance()
            while (peek().isDigit()) {
                advance()
            }
        }
        val text = source.substring(start, current)
        addToken(TokenType.NUMBER, text)
    }

    private fun stringLiteral() {
        while (peek() != '"' && !isAtEnd()) {
            if (peek() == '\n') line++
            advance()
        }
        if (isAtEnd()) {
            println("Error: Unterminated string")
            return
        }
        advance() // Consume the closing "
        val value = source.substring(start + 1, current - 1) // Exclude the quotes
        addToken(TokenType.STRING, value)
    }

    private fun addToken(type: TokenType, literal: String = "") {
        val text = source.substring(start, current)
        tokens.add(Token(type, text))
    }

    private fun match(expected: Char): Boolean {
        if (isAtEnd() || source[current] != expected) return false
        current++
        return true
    }

    private fun advance(): Char {
        current++
        return source[current - 1]
    }

    private fun peek(): Char {
        if (isAtEnd()) return '\u0000'
        return source[current]
    }

    private fun peekNext(): Char {
        if (current + 1 >= source.length) return '\u0000'
        return source[current + 1]
    }

    private fun isAtEnd() = current >= source.length
}

fun main() {
    val sourceCode = """
        stir ( h < r) {
            ingredient i : i32 = 0;
            bake (ing i = 0; i < 10; i++) {
                giveUp;
                nextPls;
            }
        }
    """.trimIndent()

    val lexer = Lexer(sourceCode)
    val tokens = lexer.tokenize()

    for (token in tokens) {
        println("${token.type} : ${token.value}")
    }
}

```


### 30 July 2023

I foudn this Structure to be bettere 
```java

void hello(){
 // COmmenet   
}
```

> This is a quote heyo