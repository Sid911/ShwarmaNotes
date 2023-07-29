### Parser interface
> List of Tokens in Lexer doc
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

Job : 
- Parsing
- Basic Error handling


### Example Implementation

```kotlin
class Parser(private val tokens: List<Token>) {
    private var current = 0

    fun parse(): List<Stmt> {
        val statements = mutableListOf<Stmt>()
        while (!isAtEnd()) {
            statements.add(declaration())
        }
        return statements
    }

    private fun declaration(): Stmt {
        return if (match(TokenType.INGREDIENT)) {
            ingredientDeclaration()
        } else {
            statement()
        }
    }

    private fun ingredientDeclaration(): Stmt {
        val name = consume(TokenType.IDENTIFIER, "Expect variable name after 'ingredient'.")
        val isFrozen = match(TokenType.FREEZE)
        consume(TokenType.COLON, "Expect ':' after variable name.")
        val type = consume(TokenType.IDENTIFIER, "Expect variable type.")

        if (isFrozen) {
            val initializer = expression()
            consume(TokenType.SEMICOLON, "Expect ';' after variable declaration.")
            return VarStmt(name, type, initializer)
        } else {
            consume(TokenType.SEMICOLON, "Expect ';' after variable declaration.")
            return VarStmt(name, type, null)
        }
    }

    private fun statement(): Stmt {
        return if (match(TokenType.STIR)) {
            whileStatement()
        } else if (match(TokenType.BAKE)) {
            forStatement()
        } else {
            expressionStatement()
        }
    }

    private fun whileStatement(): Stmt {
        consume(TokenType.LEFT_PAREN, "Expect '(' after 'stir'.")
        val condition = expression()
        consume(TokenType.RIGHT_PAREN, "Expect ')' after stir condition.")
        val body = statement()
        return WhileStmt(condition, body)
    }

    private fun forStatement(): Stmt {
        consume(TokenType.LEFT_PAREN, "Expect '(' after 'bake'.")
        val initializer = declaration()
        val condition = expression()
        consume(TokenType.SEMICOLON, "Expect ';' after loop condition.")
        val increment = expression()
        consume(TokenType.RIGHT_PAREN, "Expect ')' after 'bake' clauses.")
        val body = statement()
        return ForStmt(initializer, condition, increment, body)
    }

    private fun expressionStatement(): Stmt {
        val expr = expression()
        consume(TokenType.SEMICOLON, "Expect ';' after expression.")
        return ExpressionStmt(expr)
    }

    private fun expression(): Expr {
        // Currently, only handles assignment expressions
        return assignment()
    }

    private fun assignment(): Expr {
        val expr = equality()

        if (match(TokenType.EQUAL)) {
            val value = assignment()
            if (expr is VariableExpr) {
                return AssignExpr(expr.name, value)
            }
            error("Invalid assignment target.")
        }

        return expr
    }

    // Implement other expression parsing functions like 'equality', 'comparison', etc.

    // Helper functions

    private fun match(vararg types: TokenType): Boolean {
        for (type in types) {
            if (check(type)) {
                advance()
                return true
            }
        }
        return false
    }

    private fun check(type: TokenType): Boolean {
        return if (isAtEnd()) {
            false
        } else {
            peek().type == type
        }
    }

    private fun advance(): Token {
        if (!isAtEnd()) current++
        return previous()
    }

    private fun consume(type: TokenType, message: String): Token {
        if (check(type)) return advance()
        throw error(message)
    }

    private fun isAtEnd(): Boolean {
        return peek().type == TokenType.EOF
    }

    private fun peek(): Token {
        return tokens[current]
    }

    private fun previous(): Token {
        return tokens[current - 1]
    }

    private fun error(message: String): ParseError {
        return ParseError(message)
    }
}

data class ParseError(val message: String)

```