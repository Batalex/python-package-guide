# Document the code in your package's API using docstrings

## What is an API?
API stands for **A**pplied **P**rogramming **I**nterface. When 
discussed in the context of a (Python) package, the API refers to 
the interface and tools that you, as a package user, use in a package. 

A simple example of a package API element:
For instance, a package might have a function called `add_numbers()` 
that adds up a bunch of numbers. To add up numbers, you as the user 
simply call `add_numbers(1,2,3)` and the package function calculates the value and returns `6`. In using the `add_numbers` function, a user is 
using the package's API. 

 Package API's can consist of functions and/or classes (or object) that provide an easier-to-user interface (the API) for a user. 

## What is a docstring and how does it relate to documentation? 
In Python a docstring refers to text in a function, method or class 
that describes what the function does and its inputs, outputs and what it 
returns.

The docstring is thus important for:

* When you, as a user, call `help()` e.g. `help(add_numbers)` in Python, it returns the elements in your docstring to help guide a user towards using the function more effectively. 
* When you build your package's documentation, the docstrings can be also used to automagically create full API documentation that provides a clean view of all functions methods and classes in a package.  

```{tip}
Example API Documentation (Documentation for all functions and classes in a package)
* [View example high level API documentation for the Verde package. This page lists every function and class in the package along with a brief explanation of what it does](https://www.fatiando.org/verde/latest/api/index.html)
* [You can further dig down to see what a specific function does within the package by clicking on an API element](https://www.fatiando.org/verde/latest/api/generated/verde.grid_coordinates.html#verde.grid_coordinates)
```

## Python package API documentation 

API documentation refers to explanation about the function, inputs and outputs 
of every (*within reason*) function, class, method in your package. API documentation
in python requires that you use a docstring for each class, function or method that:

* Explains what the function, method or class does 
* Explains what every input and output variable's (type) is (ie. `string`, `int`, `np.array`)
* Explains the expected output `return` of the object, method or function.

### Python docstring best practices 

There are several Python docstring formats that you can chose to use when documenting 
your package including:

* [NumPy-style](https://numpydoc.readthedocs.io/en/latest/format.html#docstring-standard)
* [google style](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html) 
* [reST style](https://sphinx-rtd-tutorial.readthedocs.io/en/latest/docstrings.html) 

<!-- https://peps.python.org/pep-0287/ - 2002 pep 287-->
We suggest using [NumPy-style docstrings](https://numpydoc.readthedocs.io/en/latest/format.html#docstring-standard) for your 
Python documentation because:

* NumPy style docstrings are human readable (unlike reST which is harder to quickly scan and takes up more lines of code in your modules)
* NumPy format docstrings are core to the scientific Python ecosystem and defined in the [NumPy style guide](https://numpydoc.readthedocs.io/en/latest/format.html). Thus you will find them widely used there. 

```{tip}
If you are using `NumPy format` docstrings, be sure to include the [sphinx napoleon 
extension](https://www.sphinx-doc.org/en/master/usage/extensions/napoleon.html) in your documentation `conf.py` file. This extension allows sphinx 
to properly read and format NumPy format docstrings. 
```

### Docstring examples Better and Best 

Below is a good example of a well documented function. Notice that this 
function's docstring describes every function inputs and the function's output 
(or return value). The description of the function is short and to the point 
(2 to 3 sentences). And the return of the function is specified. 

```Python
def extent_to_json(ext_obj):
    """Convert bounds to a shapely geojson like spatial object.
    This format is what shapely uses. The output object can be used
    to crop a raster image.

    Parameters
    ----------
    ext_obj : list or geopandas.GeoDataFrame
        If provided with a `geopandas.GeoDataFrame`, the extent
        will be generated from that. Otherwise, extent values
        should be in the order: minx, miny, maxx, maxy.

    Returns
    -------
    extent_json: A GeoJSON style dictionary of corner coordinates
    for the extent
        A GeoJSON style dictionary of corner coordinates representing
        the spatial extent of the provided spatial object.
    """
```

<!-- I can't seem to get doc targets across pages to work-->
(docstring_best_practice)=
### Best - a docstring with example use of the function

This example contains an example of using the function that is also tested in 
sphinx using [doctest](https://docs.python.org/3/library/doctest.html).

```Python
def extent_to_json(ext_obj):
    """Convert bounds to a shapely geojson like spatial object.
    This format is what shapely uses. The output object can be used
    to crop a raster image.

        Parameters
    ----------
    ext_obj : list or geopandas.GeoDataFrame
        If provided with a `geopandas.GeoDataFrame`, the extent
        will be generated from that. Otherwise, extent values
        should be in the order: minx, miny, maxx, maxy.
    Return
    ------
    extent_json : A GeoJSON style dictionary of corner coordinates
    for the extent
        A GeoJSON style dictionary of corner coordinates representing
        the spatial extent of the provided spatial object.
    
    Example
    -------
    Convert a `geopandas.GeoDataFrame` to an extent dictionary:

    >>> import geopandas as gpd
    >>> import earthpy.spatial as es
    >>> from earthpy.io import path_to_example

	We start by loading a Shapefile.

    >>> rmnp = gpd.read_file(path_to_example('rmnp.shp'))

	And then use `extent_to_json` to do the conversion from `shp` to
    `geopandas.GeoDataFrame`.

    >>> es.extent_to_json(rmnp)
    {'type': 'Polygon', 'coordinates': (((-105.4935937, 40.1580827), ...),)}

    """

```

```{figure} /images/sphinx-rendering-extent-to-json-earthpy.png
---
name: directive-fig
width: 80%
---
Using the above NumPy format  docstring in sphinx, the autodoc extension will 
create the about documentation section for the `extent_to_json` function. The 
output of the `es.extent_to_json(rmnp)` command can even be tested using 
doctest adding another quality check to your package. 
```


## Using doctest to run docstring examples in your package's methods and functions
<!-- This link isn't working no matter how i create the target. not sure 
why -->
Above, we provided some examples of good, better, best docstring formats. If you are using Sphinx to create your docs, you can add the [doctest](https://www.sphinx-doc.org/en/master/usage/extensions/doctest.html) extension to your Sphinx build. Doctest provides an additiona; check for docstrings with example code in them. 
Doctest runs the example code in your docstring `Examples` checking 
that the expected output is correct. Similar to running
tutorials in your documentation, `doctest` can be a useful step that 
assures that your package's code (API) runs as you expect it to.

```{note} 
It's important to keep in mind that examples in your docstrings 
help users using your package. Running `doctest` on those examples provides a 
check of your package's API. doctest ensures that the functions and methods in your package 
run as you expect them to. Neither of these items replace a separate, 
stand-alone test suite that is designed to test your package's core functionality 
across operating systems and Python versions. 
```

Below is an example of a docstring with an example. 
doctest will run the example below and test that if you provide 
`add_me` with the values 1 and 3 it will return 4.  


```python
def add_me(aNum, aNum2):
    """A function that prints a number that it is provided. 
    
    Parameters
    ----------
    aNum : int
        An integer value to be printed
    
    Returns 
    -------
        Prints the integer that you provide the function.
    
    """
   return aNum + aNum2

Examples
--------
Below you can see how the `print_me` function will print a number that 
you provide it.

>>> add_me(1+3)
4

```