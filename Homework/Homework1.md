# Homework 1
#### Justin Lyogky
### Adding test cases for parse_term
We hope to test both multiplication and division of 2 or more factors.
``` python
def test_parse_term():
    """
    term = factor { "*"|"/" factor }
    """
    print("testing parse_term")

    tokens = tokenize("5*2")
    ast, tokens = parse_term(tokens)
    assert ast == {
        "tag": "*",
        "left": {
            "tag": "number",
            "value": 5,
            "position": 0,
        },
        "right": {
            "tag": "number",
            "value": 2,
            "position": 2,
        },
    }

    tokens = tokenize("1/7")
    ast, tokens = parse_term(tokens)
    assert ast == {
        "tag": "/",
        "left": {
            "tag": "number",
            "value": 1,
            "position": 0,
        },
        "right": {
            "tag": "number",
            "value": 7,
            "position": 2,
        },
    }

    tokens = tokenize("1/7*8/1")
    ast, tokens = parse_term(tokens)
    assert ast == {
        'tag': '/', 
        'left': {
            'tag': '*', 
            'left': {
                'tag': '/', 
                'left': {'tag': 'number', 'value': 1, 'position': 0}, 
                'right': {'tag': 'number', 'value': 7, 'position': 2}}, 
            'right': {'tag': 'number', 'value': 8, 'position': 4}}, 
        'right': {'tag': 'number', 'value': 1, 'position': 6}}

```



### Adding test cases for parse_expression
We hope to test adding, subtracting, multiplying, and dividing of multiple terms together and test for the proper evaluation order.
``` python
def test_parse_expression():
    """
    expression = term { "+"|"-" term }
    """
    print("testing parse_expression")

    tokens = tokenize("1*7+1*4")
    ast, tokens = parse_expression(tokens)
    assert ast == {
        "tag": "+",
        "left": {
            "tag": "*",
            "left": {
                "tag": "number",
                "value": 1,
                "position": 0
            },
            "right": {
                "tag": "number",
                "value": 7,
                "position": 2,
            },
        },
        "right": {
            "tag": "*",
            "left": {
                "tag": "number",
                "value": 1,
                "position": 4,
            },
            "right": {
                "tag": "number",
                "value": 4,
                "position": 6,
            },
        },
    }

    tokens = tokenize("3*5-6/8")
    ast, tokens = parse_expression(tokens)
    assert ast == {
        "tag": "-",
        "left": {
            "tag": "*",
            "left": {
                "tag": "number",
                "value": 3,
                "position": 0
            },
            "right": {
                "tag": "number",
                "value": 5,
                "position": 2,
            },
        },
        "right": {
            "tag": "/",
            "left": {
                "tag": "number",
                "value": 6,
                "position": 4,
            },
            "right": {
                "tag": "number",
                "value": 8,
                "position": 6,
            },
        },
    }


    tokens = tokenize("2+1-3+5")
    ast, tokens = parse_expression(tokens)
    assert ast == {
        'tag': '+', 
        'left': {'tag': '-', 
            'left': {'tag': '+', 
                'left': {'tag': 'number', 'value': 2, 'position': 0}, 
                'right': {'tag': 'number', 'value': 1, 'position': 2}}, 
            'right': {'tag': 'number', 'value': 3, 'position': 4}}, 
        'right': {'tag': 'number', 'value': 5, 'position': 6}
    }
```


### Additional tests
These are additional test cases for the other test functions.
##### Testing parse_factor
``` python
for s in ["-5", "(-5)", "5"]:
        assert parse_factor(tokenize(s)) == parse_simple_expression(tokenize(s))
```
##### Testing parse_simple_expression
``` python
tokens = tokenize("-(5)")
    ast, tokens = parse_simple_expression(tokens)
    assert ast == {
        "tag": "negate",
        "value": {"position": 2, "tag": "number", "value": 5},
    }
```



### Conclusion
We have added more test cases to our parser to ensure the validility, execution, and response of functions. We tested parsing two terms with
multiplication and division as well as multiple terms. We tested parsing expressions with addition and subtraction, as well as multiplication and 
division. We also tested a combination of these operators to ensure the proper order of executing these operators. We then added additional test cases to 
the other functions. 
