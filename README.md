simpleValidator
===============

[![Build Status](https://travis-ci.org/kyosuken/simpleValidator.png?branch=master)](https://travis-ci.org/kyosuken/simpleValidator)


A small, extensible python 2 (and 3 compatible !) library to deal with web validations !

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

print(v.fails())
### outputs True

print(v.errors())
### outputs ['name is not equal to 1 !']

```


Rules List
==========

    * required, field to validate must contain a value
    * email, field must be a valid email
    * alpha, field must contain alphabetical characters only
    * alpha_num, field must contain alphabetical characters and/or numbers
    * alpha_dash, field must contain alphabetical characters, numbers, dashes and underscores
    * numeric, field must contain a numerical value
    * integer, field must be an integer only
    * posinteger, field must be a positive integer only
    * min, depending on field:
        - string, size must be at least of min value (ex min = 5, "mystring" is valid)
        - numerical, value must be at least higher or equal to min value (ex min = 22, 39 is valid)
    * max, depending on field:
        - string, size must at most be of max value (ex max = 10, "hello world" is NOT valid)
        - numerical, value must be lower or equal than max value (ext max = 10, 12 is NOT valid)
    * between, depending on value:
        - string, size must be between the boundary values (ex between = (5,10), "hello !" is valid)
        - numerical, value must be between the 2 values 
    * ip4, field must be a valid ipv4 address
    * ip6, field must be a valid ipv6 address
    * date, field must be a valid date, corresponding to a specific template
    * url, field must be a valid url (https, and port are allowed)


To Do
-----

    - Add more validation rules with time...
    - Make more tests on the validator class itself...





Is it UnitTested ?
------------------

Of course ! please check rules_tests.py and validator_tests.py for tests examples. Simply run them with python rules_tests.py or python validator_tests.py