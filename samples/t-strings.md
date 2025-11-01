# Using Template String Literals (“t‑strings”) in Python 3.14

## Tabel of Contents

1. What are t-strings？
2. Basics usage
3. Why use t‑strings？
4. Practical example
5. Summary

## 1. What are t-strings?

In Python 3.14 a new form of string literal has been introduced under **PEP 750: template string literals**, or t‑strings. They look similar to f‑strings but the expression inside the braces is _not immediately evaluated_. Instead they produce a template object that can later be filled or processed. This gives you more control and opens possibilities for safe templating, custom string processing, sanitization, or deferred interpolation.

## 2. Basics usage

Here is a simple example of using t-strings:

```python
name = 'John'
site = 'Real Python'

t = t'Hello, {name}! Welcome to {site}.'
print(t)
# Template(strings=('Hello, ', '! Welcome to ', '.'), interpolations=(Interpolation('John', 'name', None, ''), Interpolation('Real Python', 'site', None, '')))
print(t.strings)
# ('Hello, ', '! Welcome to ', '.')
print(t.values)
# ('John', 'Real Python')
```

Unlike f-strings, t-strings are not evaluated immediately. Instead, they produce a template object that can later be filled or processed. This gives you more control and opens possibilities for safe templating, custom string processing, sanitization, or deferred interpolation.

## 3. Why use t‑strings？

T-strings are a generalization of f-strings. They are more powerful and flexible than f-strings, but they are also more complex.

- Security / sanitization: If you're building a templating engine or accepting user input, deferred interpolation gives you a chance to process/sanitize placeholders before evaluation.
- Reusable templates: You can define templates once and re‑use them with different contexts.
- Separation of template and data: Keeps the structure of the string separate from the data that fills it, improving readability and maintainability.
- Better performance in some cases: Because interpolation is delayed, you avoid doing work when it's not needed.

## 4. Practical example

Here is a practical example of using t-strings:

```python
def email_template(platform_name, user_name, register_date, account_type, promo_message):
    return t"""
Subject: Welcome to {platform_name}, {user_name}!

Hi {user_name},

Thank you for joining {platform_name}. We're excited to have you on board!

Your registration date: {register_date}
Account type: {account_type}

{promo_message}

Best regards,
The {platform_name} Team
"""

def render_email(template):
    parts = []
    for item in template:
        if isinstance(item, str):
            parts.append(item)
        else:
            parts.append(item.value)
    return ''.join(parts)

print(render_email(email_template('Real Python', 'John', '2025-11-01', 'Premium', 'Join our exclusive membership program and get 10% off your first purchase!')))

# Subject: Welcome to Real Python, John!
#
# Hi John,
#
# Thank you for joining Real Python. We're excited to have you on board!
#
# Your registration date: 2025-11-01
# Account type: Premium
#
# Join our exclusive membership program and get 10% off your first purchase!
#
# Best regards,
# The Real Python Team
```

## 5. Summary

Template string literals (t‑strings) in Python 3.14 provide a new way to define strings whose interpolation is deferred, offering improved separation between template structure and data, better security/sanitization opportunities, and enhanced reusability. As you adopt Python 3.14, consider using t‑strings when you build templating logic, report generators, or any scenario where you want control over when and how interpolation occurs.
