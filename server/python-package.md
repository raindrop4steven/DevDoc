#Be Pythonic: __init__.py


This is hopefully the first in a series of posts about writing Pythonic code and explaining some common Python idioms.

So the first thing I am going to address in this series of posts is `__init__.py`.

##What is __init__.py used for?
The primary use of `__init__.py` is to **initialize Python packages**. The easiest way to demonstrate this is to take a look at the structure of a standard Python module.

	package/
	    __init__.py
	    file.py
	    file2.py
	    file3.py
	    subpackage/
	        __init__.py
	        submodule1.py
	        submodule2.py

As you can see in the structure above the inclusion of the `__init__.py` file in a directory indicates to the Python interpreter that the directory should be treated like a Python package

## What goes in __init__.py?
`__init__.py` can be an empty file but it is often used to **perform setup needed for the package(import things, load things into path, etc)**.

1. One common thing to do in your `__init__.py` is to **import selected Classes, functions, etc into the package level so they can be convieniently imported from the package**.

    In our example above we can say that file.py has the Class File. So without anything in our `__init__.py` you would import with this syntax:

		from package.file import File

	However you can import File into your `__init__.py` to make it available at the package level:
	
		# in your __init__.py
		from file import File
	
		# now import File from package
		from package import File

2. Another thing to do is **at the package level make subpackages/modules available** with the `__all__` variable. When the interpeter sees an `__all__` variable defined in an `__init__.py` it imports the modules listed in the `__all__`variable when you do:

		from package import *
`__all__`is a list containing the names of modules that you want to be imported with import * so looking at our above example again if we wanted to import the submodules in subpackage the `__all__` variable in `subpackage/__init__.py` would be:

		__all__ = ['submodule1', 'submodule2']
With the `__all__` variable populated like that, when you perform

		from subpackage import *
it would import submodule1 and submodule2.

As you can see `__init__.py` can be very useful besides its primary function of indicating that a directory is a module. If you have any comments or questions, hit up the comments or contact me on twitter.

If you are looking for more Python news, tips and discussion you should check out Pycoder's Weekly a weekly Python newsletter that I curate.