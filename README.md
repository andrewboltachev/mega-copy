mega-copy
=========

This is a tool that helps to copy code in bulk. Useful when developing similar features

It works with:

1. Python files in "special" (Python-language specific) mode
2. "Other" files in "simple" (format-agnostic) mode

Idea
====

Two main ideas:
* in a project code (especially of something like dashboard-like enterprise app) one might often need to "copy" one concept to another (and future make little changes to the copy). E.g. you might want to copy something called `Property Sale` to something else called `Sales Order`
* one name, being used in variable/function/class names/configs etc has multiple spellings in different places of the code (and that spelling should be preserved on copy)

If we put two ideas together this might result into following "replace map":
```json
{
    "propertysale": "salesorder",
    "property sale": "sales order",
    "property-sale": "sales-order",
    "property_sale": "sales_order",
    "PROPERTYSALE": "SALESORDER",
    "PROPERTY SALE": "SALES ORDER",
    "PROPERTY-SALE": "SALES-ORDER",
    "PROPERTY_SALE": "SALES_ORDER",
    "PropertySale": "SalesOrder",
    "Property Sale": "Sales Order",
    "Property-Sale": "Sales-Order",
    "Property_Sale": "Sales_Order",
    "Propertysale": "Salesorder",
    "Property sale": "Sales order",
    "Property-sale": "Sales-order",
    "Property_sale": "Sales_order",
    "propertySale": "salesOrder",
    "property Sale": "sales Order",
    "property-Sale": "sales-Order",
    "property_Sale": "sales_Order"
}
```


Installation
============

1. Clone this repository to anywhere in your filesystem:

```sh
git clone https://github.com/andrewboltachev/mega-copy.git
```

2. Create a virtualenv/venv or use directly (Python 3.7+ should be fine) and install requirements
For example, using virtualenvwrapper:

```sh
mkvirtualenv helper1
pip install -r requirements.txt
```

3. In a directory, accessible to your `PATH` (e.g. `/home/<user>/bin`), create a file called `mega-copy.sh` with the following content:
```sh
/home/<user>/.virtualenvs/<venv-name>/bin/python /home/<user>/<tools>/mega-copy/mega-copy.py $@
```

Which starts a script with required interpreter and params
(Adjust correspondingly for interpreter location)

4. You should now be able to use `mega-copy.sh` from your system

Usage
=====

Main syntax:
```sh
mega-copy.sh <command> <pattern1> <pattern2> <pattern3>... <replacement> [<filename>]
```

Commands, which end with `-file`, use the last parameter as filename. Other commands run on whole directory where the command is launched from

Commands themselves are described below. `pattern`s and `replacement` are strings in hyphen-concatenated lower-case, e.g.:
```sh
mega-copy.sh show property-sale sales-order
```

Python files commands
---------------------
1. `show`, `show-file`
Prints out forms from Python files that will differ if replacement map will be applied
2. `ren`, `ren-file`
Renames all the patterns with replacement in place
3. `copy`, `copy-file`
Very special — copies in place (i.e. modifying the files) class definitions which have a patterns in them into a replacement
E.g. `mega-copy.sh copy-file property-sale sales-order views.py` will copy `PropertySaleDetailView` into `SalesOrderDetailView` (and append the latter after the original one) inside of your `views.py` file

Other (plain text-based) commands
---------------------------------
All commands work only on directories (not on single files)
1. `file-show`
Print out contents of files (non-Python, e.g. JS, HTML etc), which differ when applying replace map
2. `file-ren`
Make replacements inside of files accroding to replace map in-place
3. `file-copy`
Find files that have the pattern inside of it (e.g. `property-sale-data.js`) and copy it, applying replace map to filename and contents (resulting file will be called `sales-order-data.js`)

Hints
=====

1. Play around (using `show` commands and/or normal commands and `git reset` to get back) to get better idea of what the tool is doing before solving real problems
2. When using commands that make changes, please make sure the git repo is clean (otherwise the tool will return an error). Perhaps make each change the tool did into a commit
