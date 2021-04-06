Devrae: Avrae Development Utilities
===================================

This utility supports in:
- Creating the development environment
- Support *very simplified* functions with macros
- Deploy of aliases to Avrae Workshop

Prerequisite
-----------
* Python 3
* https://avrae.io Account
* Basic File Management/Command Line Knowledge

How to Install
--------------
Clone the repository and add the devrae folder to the PATH.
You will need to have Python's `requests` module installed, to do that, run:
```python -m pip install requests```
or:
```python3 -m pip install requests```

Using Davrae
-------------

Davrae supports these commands:

`davrae init`: use this in the root folder of your projects to create Davrae's folder structure.
Devrae expects a certain folder structure to be in place for it work properly:
*aliases:* contains the aliases that will be deployed to your Avrae account.
*config:* contains the `avrae-ids.json` file that lists the aliases and gvars, and their IDs that are necessary for deployment to Avrae.
*functions:* contains reusable functions that can be used in your Avrae aliases. The functions are compiled into your aliases, and the compiled version is deployed to Avrae.
*gvars:* contains gvars that are deployed to Avrae automatically through Davrae.
*out:* this is folder where Devrae will write the compiled aliases.

`davrae compile`: compiles all aliases in the *aliases* folder, by injecting your functions into them.
The compiled aliases are placed in the `out` folder.

`davrae deploy`: deploys the aliases and gvars configured in the `config/avrae-ids.json` file.

`davrae get-ids <collection id>`: this command takes a collection id and prints the alias IDs for the objects in your collection.
These ids need to be placed in the `config/avrae-ids.json` for deployment.

Using Devrae Functions
------------------------

Unfortunately at this time Avrae does not support writing Pythong functions/methods in the customization of Aliases.
With Devrae, you can write simple functions that are injected into your aliases via macros through a compilation process.

Function parameters are accessed with `__p0__`, `__p1__`, `__p2__` and so on.
The function's return value is stored in the variable `__result__`.
The `return` keyword is not supported. Do not use it inside your functions or your code will not behave as expected.

Example
> functions/char_eval.func
```python
_name = __p0__
_age = __p1__

if _age >= 18:
  __result__ = "You character is older than 18 years old!"
else:
  __result__ = "Your character is still a youngling!"
```

> aliases/charage.alias
```python
!alias charage embed
<drac2>
char_age = int(%1%)
char_name = character.name()
desc = #! devrae_f char_eval (#P char_name #P char_age) !#
</drac2>
-t "What is your character age?"
-desc dec
```

Now you need to compile your aliases, so Devrae can inject the functions into them.
To do so, from the root of your project, run:

`devrae compile`

Look into your `out` folder, and the compiled version will be there!

Caveats
--------

Devrae functions are a very limited and simple method to introduce some level of reusability to Avrae's Draconic language.
- Be mindful of the names of the variables, as the scope of the functions share the same scope as the alias code they are injected in.
- Because of that, it is advisable that you use hidden variable names (as per Python's convention) in your function-scoped variables, as in the example above.
- Do not use the `return` keyword inside your functions.

Acknowledgments
---------------

Thanks to [DTurtle](https://github.com/1drturtle) for the code to deploy gvars and aliases, and the code to retrieve collection object IDs.
DTurtle develops amazing tools that automates Avrae deploys via GitHub workflows, as well as great Avrae Aliases.
Check it out on [their GitHub page!](https://github.com/1drturtle)
