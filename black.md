# [Black Code Style](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html)

## Line Wraper
일반적으로 한 줄에 하나의 완전한 표현이나 간단한 문구로 표기 하는 것을 원칙으로 합니다.

- 세로 공백이 할당된 라인과 맞지 않는 경우, 첫 라인에 넣는다.
- 위 경우 동작하지 않으면, 별도의 줄에 넣는다.
- 백슬레쉬 보다 괄호를 우선으로 하며, 백슬래시 발견시 제거 
  
    > black formatter는 백슬러시가 코드 가독성을 떨어뜨리는 요인으로 생각하고 대부분의 경우 백슬러시는 제거함.
  

### Example 1.

``` python
# in:

j = [1,
     2,
     3
]

# out:

j = [1, 2, 3]
```   
### Example 2.

```python
# in:

ImportantClass.important_method(exc, limit, lookup_lines, capture_locals, extra_argument)

# out:

ImportantClass.important_method(
    exc, limit, lookup_lines, capture_locals, extra_argument
)
```

### Example 3.
```python
# in:

def very_important_function(template: str, *variables, file: os.PathLike, engine: str, header: bool = True, debug: bool = False):
    """Applies `variables` to the `template` and writes to `file`."""
    with open(file, 'w') as f:
        ...

# out:

def very_important_function(
    template: str,
    *variables,
    file: os.PathLike,
    engine: str,
    header: bool = True,
    debug: bool = False,
):
    """Applies `variables` to the `template` and writes to `file`."""
    with open(file, "w") as f:
        ...
```

### Example 4.
```
# in:

if some_short_rule1 \
  and some_short_rule2:
      ...

# out:

if some_short_rule1 and some_short_rule2:
  ...


# in:

if some_long_rule1 \
  and some_long_rule2:
    ...

# out:

if (
    some_long_rule1
    and some_long_rule2
):
    ...
```

## Line length (줄 길이)
일반적으로 formatter에서 많이 사용하는 길이는 80자 이며, black formatter에서는 기본값을 88자 설정하고 있다. 

- --line-length 옵션으로 변경 가능.
- 정적분석기를 사용할 때, configuration 주의


## Empty lines (공백)
기본적으로 수직 공백을 제거한다. 단, 함수 내부에 있거나 편집기나 모듈 수준에서의 공백은 허용 합니다. 
- 그 외 관련 참고 내용 [PEP 257](https://www.python.org/dev/peps/pep-0257/#multi-line-docstrings)

## Comments (주석)
서식을 지정하지 않는다. 그러나 코드와 같은 라인의 주석이 있거나 특정 간격 규칙이 필요한 경우 등 예외 경우가 있습니다. 자세한건 [AST before and after formatting](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#ast-before-and-after-formatting)

## Trailing commas (문장 끝 쉼표)
기본적으로 분할된 표현식 문장 끝에 쉼표를 추가합니다. 그러나 예외적으로 `*`, `*args`, `**kargs` 이 경우에는 python 3.6 이상 에서만 쉼표 추가를 합니다. 

## Strings
black formatter 에서는 `'`보다 `"`를 선호한다. 백슬래시나 escape 발생하지 않는 이상 큰따옴표를 우선시 합니다. 

그 밖에 자세한 사항은  [PEP 257](https://www.python.org/dev/peps/pep-0257/#multi-line-docstrings) 및 [Black Code Style](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html) 참고

## Numeric literals (숫자 형태의 문자)
숫자 자체에 대문자를 사용하도록 한다.
- 0`x`AB 대신 0`X`AB
- 1`e`10 대신 1`E`10


## Line breaks & binary operators (줄 바꿈 및 이항 연산자)
[PEP 8](https://www.python.org/dev/peps/pep-0008/#should-a-line-break-before-or-after-a-binary-operator) 방식을 준수한다. 단항연산자 처럼 하나의 연산자로 취급함 

## Slices [`:`]
슬라이싱 양쪽에 균일한 공백을 남기는 것을 권장한다. [PEP 8](https://www.python.org/dev/peps/pep-0008/#should-a-line-break-before-or-after-a-binary-operator) 

> 이 동작은 Flake8과 같은 정적분석기에서 경고를 발생시킬 수 있습니다. PEP 8과 호환되지 않으므로 Flake8 에 이러한 경고를 무시하도록 알려야 합니다.E203 whitespace before ':'E203


## Parentheses (괄호)
Python에서 선택적으로 사용되는 경우가 있다. 
- if (...):
- while (...):
- for (...) in (...):
- assert (...), (...)
- from X import (...)
- example
  - target = (...)
  - target: type = (...)
  - some, *un, packing = (...)
  - augmented += (...)

이 경우 내부 표현식에 추가로 분할할 구분 기호가 없으면 괄호를 제거합니다.

black formatter은 명확성을 위해 또는 추가 코드 구성을 위해 추가로 중첩된 괄호를 추가하거나 제거하지 않습니다.

```python
return not (this or that)
decision = (maybe.this() and values > 0) or (maybe.that() and values < 0)
```

## 생략
- Call chains
- Typing stub files

---

## 기타 
### 정적 분석기 flake8 에서 configuration 용어

```
[flake8]
;W191:indentation contains tabs
;E101:indentation contains mixed spaces and tabs
;E126:continuation line over-indented for hanging indent
;E131:continuation line unaligned for hanging indent
;E203:whitespace before ':'
;E226:missing whitespace around arithmetic operator
;E231:missing whitespace after ':'
;E261:at least two spaces before inline comment
;E265:block comment should start with '# '
;E302:expected 2 blank lines, found 0
;E701:multiple statements on one line (colon)
;C901: ... is too complex (nn)

ignore = W191,E101,E126,E131,E203,E226,E231,E261,E265,E301,E302,E701,C901
max-line-length = 160
exclude = tests/*
max-complexity = 10
```

black formatter 사용시 flaske8 configuration 예시

```
[flake8]
ignore = E203, E266, E501, W503
# line length is intentionally set to 80 here because black uses Bugbear
# See https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#line-length for more details
max-line-length = 80
max-complexity = 18
select = B,C,E,F,W,T4,B9

```

sample.py
```python
'''
-l : 한 라인에 최대 글자 수 (초기값 88)

—diff : 파일을 변경하지 않고 변경되는 부분을 콘솔로 보여준다.

—color : —diff를 사용했을 때 변경점에 색을 입힌다.

example 
- black {파일명 또는 폴더명} -l 80 --diff --color
'''

from seven_dwwarfs import Grumpy, Happy, Sleepy, Bashful, Sneezy, Dopey, Doc
x = {  'a':37,'b':42,
'c':927}

x = 123456789.123456789E1234567891111111111111

if very_long_variable_name is not None and \
 very_long_variable_name.field > 0 or \
 very_long_variable_name.is_debug:
 z = 'hello '+'world'
else:
 world = 'world'
 a = 'hello {}'.format(world)
 f = rf'hello {world}'
if (this
and that): y = 'hello ''world'#FIXME: https://github.com/python/black/issues/26
class Foo  (     object  ):
  def f    (self   ):
    return       37*-2
  def g(self, x,y=42):
      return y
def f  (   a: List[ int ]) :
  return      37-a[42-u :  y**3]
def very_important_function(template: str,*variables,file: os.PathLike,debug:bool=False,):
    """Applies `variables` to the `template` and writes to `file`."""
    with open(file, "w") as f:
     ...
# fmt: off
custom_formatting = [
    0,  1,  2,
    3,  4,  5,
    6,  7,  8,
]
# fmt: on
regular_formatting = [
    0,  1,  2,
    3,  4,  5,
    6,  7,  8,
]
```