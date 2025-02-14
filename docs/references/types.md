# Types

## Output types

Output types are generated from models. `auto` type is used for field type auto resolution. Relational fields are described by referencing to other type genedated from Django model. Many to many relation is described by using List type. `strawberry.django` will automatically generate resolvers for relational fields. More information about that can be read from [resolvers](resolvers.md) page.

```python
from strawberry.django import auto
from typing import List

@strawberry.django.type(models.Fruit)
class Fruit:
    id: auto
    name: auto
    color: 'Color'

@strawberry.django.type(models.Color)
class Color:
    id: auto
    name: auto
    fruits: List[Fruit]
```

## Input types

Input types can be generated from models by using `strawberry.django.input` decorator. First parameter is always model, where the type is converted from.

```python
@strawberry.django.input(models.Fruit)
class FruitInput:
    id: auto
    name: auto
    color: 'ColorInput'
```

Partial input type, which all fields are optional, is generated by adding `partial=True` parameter for `input`. Partial input can be generated from existing input type by using class inheritance.

```python
@strawberry.django.input(models.Color, partial=True)
class FruitPartialInput(FruitInput):
    color: List['ColorPartialInput']

@strawberry.django.input(models.Color, partial=True)
class ColorPartialInput:
    id: auto
    name: auto
    fruits: List[FruitPartialInput]
```
