Also is part of aiogram. In aiogram has some additional features

### Installation:
`pip install magic_filter`

### How to use:
You can create custom filter, for example:
```python
from dataclasses import dataclass
from magic_filter import F

@dataclass
class Person:
	name: str
	age: int

p = Person(name='John')

F.name.resolve(p)
# True

F.age.resolve(p)
# False

(F.name == 'Jahne').resolve(p)
# False

```

When we specify `F` as a filter, the Aiogram passes the update object (e.g., `Message`) to the `resolve()` method and then invokes it.



The `MagicFilter` object is callable, supports [some actions](https://docs.aiogram.dev/en/dev-3.x/dispatcher/filters/magic_filters.html#magic-filter-possible-actions) and memorize the attributes chain and the action which should be checked on demand.

So that’s mean you can chain attribute getters, describe simple data validations and then call the resulted object passing single object as argument, for example make attributes chain `F.foo.bar.baz` then add action ‘`F.foo.bar.baz == 'spam'` and then call the resulted object - `(F.foo.bar.baz == 'spam').resolve(obj)`

## Possible actions

Magic filter object supports some of basic logical operations over object attributes

### Exists or not None

Default actions.

```python
F.photo  # lambda message: message.photo
```

### Equals

```python
F.text == 'hello'  # lambda message: message.text == 'hello'
F.from_user.id == 42  # lambda message: message.from_user.id == 42
```

### Is one of

Can be used as method named `in_` or as matmul operator `@` with any iterable

```python
F.from_user.id.in_({42, 1000, 123123})  # lambda query: query.from_user.id in {42, 1000, 123123}
F.data.in_({'foo', 'bar', 'baz'})  # lambda query: query.data in {'foo', 'bar', 'baz'}
```

### Contains

```python
F.text.contains('foo')  # lambda message: 'foo' in message.text
```

### String startswith/endswith

Can be applied only for text attributes

```python
F.text.startswith('foo')  # lambda message: message.text.startswith('foo')
F.text.endswith('bar')  # lambda message: message.text.startswith('bar')
```

### Regexp

```python
F.text.regexp(r'Hello, .+')  # lambda message: re.match(r'Hello, .+', message.text)
```

### Custom function

Accepts any callable. Callback will be called when filter checks result

```python
F.chat.func(lambda chat: chat.id == -42)  # lambda message: (lambda chat: chat.id == -42)(message.chat)
```

### Inverting result

Any of available operation can be inverted by bitwise inversion - `~`

```python
~(F.text == 'spam')  # lambda message: message.text != 'spam'
~F.text.startswith('spam')  # lambda message: not message.text.startswith('spam')
```

### Combining

All operations can be combined via bitwise and/or operators - `&`/`|`

```python
(F.from_user.id == 42) & (F.text == 'admin')
F.text.startswith('a') | F.text.endswith('b')
(F.from_user.id.in_({42, 777, 911})) & (F.text.startswith('!') | F.text.startswith('/')) & F.text.contains('ban')
```

### Attribute modifiers - string manipulations

Make text upper- or lower-case

Can be used only with string attributes.

```python
F.text.lower() == 'test'  # lambda message: message.text.lower() == 'test'
F.text.upper().in_({'FOO', 'BAR'})  # lambda message: message.text.upper() in {'FOO', 'BAR'}
F.text.len() == 5  # lambda message: len(message.text) == 5
```

### Get filter result as handler argument

This part is not available in _magic-filter_ directly but can be used with _aiogram_

```python
from aiogram import F

...

@router.message(F.text.regexp(r"^(\d+)$").as_("digits"))
async def any_digits_handler(message: Message, digits: Match[str]):
    await message.answer(html.quote(str(digits)))
```

## Usage in _aiogram_

```python
@router.message(F.text == 'hello')
@router.inline_query(F.data == 'button:1')
@router.message(F.text.startswith('foo'))
@router.message(F.content_type.in_({'text', 'sticker'}))
@router.message(F.text.regexp(r'\d+'))

...

# Many others cases when you will need to check any of available event attribute
```



[[Python]]
[[Python Libraries]]
[[Filters]]







