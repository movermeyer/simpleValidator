simpleValidator
===============

[![Build Status](https://travis-ci.org/kyosuken/simpleValidator.png?branch=master)](https://travis-ci.org/kyosuken/simpleValidator)


A small, extensible python 2 library to deal with web validations !

simpleValidator (or just Validator actually), comes from the need of having a simple and straightforward validation library in python.

Sure some heavy players already exists, like WTForm, but take it as both a challenge and having somefun :), it is also inspired by the simplicity of [Laravel](https://github.com/laravel/laravel)'s own validator class

The library is standalone, rules are built the python way, in a module and easy to implement, the library is extensible as well !

Where to use it ? in [Flask](https://github.com/mitsuhiko/flask) for example for people who need very simple validations

N.B. : while i have years of experiences in php, i am mostly a newbie in python, this was a good idea to test on, and my very first OSS project as well :)

Use case:
---------

```python
from validator import Validator

my_items_to_test = {
    'name': 'myusername',
    'email': 'myfakeemail@fakedomain.com',
}

my_validation_rules = {
    'name': 'required',
    'email': 'required|email',
}

v = Validator()
v.make(fields = my_items_to_test, rules = my_validation_rules)

### alternatively from the class constructor :
v = Validator(fields = my_items_to_test, rules = my_validation_rules)

### returns True if the validation failed, False if passed
if v.fails():
    # do something
else: 
    # do something else

### returns a list of error messages
v.errors() 

### returns the list of failed validation only (no error message)
v.failed() 
```

Custom Validation !
-------------------

simpleValidator is extensible at runtime ! you can add in your own validation rules and messages !

```python
from validator import Validator

my_items_to_test = {
    'name': 'myusername',
    'email': 'myfakeemail@fakedomain.com',
}

my_validation_rules = {
    'name': 'required|mycustomrule',
    'email': 'required|email',
}

my_validation_messages = {
    'mycustomrule': '{0} is not equal to 1 !'
}

def mycustomrule(value):
    return value == 1

v = Validator()
v.extend({'mycustomrule': mycustomrule})

v.make(fields = my_items_to_test, rules = my_validation_rules, messages = my_validation_messages)

print v.fails()
### outputs True

print v.errors()
### outputs [name is not equal to 1 !]

```

To Do
-----

    - Add more validation rules ! (ip, size, validurl, etc, etc)
    - Make more tests as needed !
    - Make the code less ugly in some parts ? 

Is it UnitTested ?
------------------

Of course ! please check rules_tests.py and validator_tests.py for tests examples. Simply run them with python rules_tests.py or python validator_tests.py