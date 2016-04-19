## @decorator

http://simeonfranklin.com/blog/2012/jul/1/python-decorators-in-12-steps/

## Same as
f = decorator(f)

@decorator
def f():
   pass
   
## functools @wraps, 可以将被封装函数的__name__, __doc__等都付给封装函数

    >>> from functools import wraps
    >>> def my_decorator(f):
    ...     @wraps(f) # copy __name__, __doc__ etc to my_decorator
    ...     def wrapper(*args, **kwds):
    ...         print 'Calling decorated function'
    ...         return f(*args, **kwds)
    ...     return wrapper  # wrapper already decorated f, returned is wrapped f.
    ...
    >>> @my_decorator
    ... def example():
    ...     """Docstring"""
    ...     print 'Called example function'
    ...
    ... # same as 
    ... def example():
    ...    pass
    ... example = my_decorator(example)
    ... not the same example any more.
    >>> example()
    Calling decorated function
    Called example function
    >>> example.__name__
    'example'
    >>> example.__doc__
    'Docstring'
    

## @functools.wraps(f):

    def my_decorator(f):
        @wraps(f)
        def wrapper(*args, **kwargs):
            return f(*args, **kwargs)
        
        return wrapper
        
        # same as
        update_wrapper(wrapper, f, xxx)

1. Returns a decorator that invokes update_wrapper().
2. the decorated function as the wrapper argument and the arguments to wraps() as the
 remaining arguments. Default arguments are as for update_wrapper().