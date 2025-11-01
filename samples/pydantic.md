# Validating and Managing Data with Pydantic

## Table of Contents

1. Introduction to Pydantic
2. Getting Started with Pydantic
3. Defining Models with Pydantic
4. Validating Data with Pydantic
5. Summary

## 1. Introduction to Pydantic

Pydantic is the most widely used data validation library for Python.
It is designed to be simple, fast, and extensible.

Without Pydantic, you may write code like this:

```python
def create_user(name, age): -> dict:
    if not isinstance(name, str):
        raise TypeError('Name must be a string')
    if not isinstance(age, int):
        raise TypeError('Age must be an integer')
    if age < 0:
        raise ValueError('Age must be positive')
    return {'name': name, 'age': age}

user = create_user('John', 30)
```

With Pydantic, you can write code like this:

```python
from pydantic import BaseModel, Field
class User(BaseModel):
    name: str
    age: int = Field(..., gt=0)

user = User(name='John', age=30)
```

Pydantic will automatically validates the data and raises an error if the data is invalid.

## 2. Getting Started with Pydantic

To get started with Pydantic, you need to install it using pip/uv:

```bash
pip install pydantic
uv add pydantic
```

## 3. Defining Models with Pydantic

To define a model with Pydantic, you need to create a class that inherits from pydantic.BaseModel.
The class defines the fields of the model. Each field is defined using a type annotation.

```python
from pydantic import BaseModel, Field, EmailStr, ConfigDict


class User(BaseModel):
    id: int
    name: str = 'John'
    age: int = Field(..., gt=0)
    email: EmailStr # need to install 'pydantic[email]'

    model_config = ConfigDict(str_max_length=10)
```

## 4. Validating Data with Pydantic

Pydantic provides a lot of built-in validators. You can use them to validate the data.
Here is an example for _After_ validators:

```python
from typing import Annotated

from pydantic import AfterValidator, BaseModel, ValidationError


def greater_than_zero(value: int) -> int:
    if value <= 0:
        raise ValueError(f'{value} shoud be greater than zero')
    return value


class User(BaseModel):
    age: Annotated[int, AfterValidator(greater_than_zero)]


try:
    User(age=-1)
except ValidationError as err:
    print(err)
    """
    1 validation error for User
    number
      Value error, -1 shoud be greater than zero [type=value_error, input_value=-1, input_type=int]
    """
```

### 5. Summary

Hundreds of organisations and packages are using Pydantic. It is a powerful tool for validating and managing data.
However, there are many detailed features that are not covered in this article. You can find more information in the [official documentation](https://docs.pydantic.dev/lates).
