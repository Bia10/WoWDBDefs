# WoWDBDefs 
This repository will have an up to date location for column/field definitions for database files used in World of Warcraft.

Project goals:
- Updated database definitions for all versions of World of Warcraft
- Human readable
- Machine readable

Cool stuff we might end up doing if this gets enough traction:
- Repository will feed automated updates on WoWDev.wiki
- Pull requests are automatically checked for validity

## Format
The format in which these files will be built up [is still up for discussion](https://github.com/Marlamin/WoWDBDefs/issues/1). Current proposal is as follows:

### Column definitions
List of column definitions at the start of the file. This is defined once per column at the start to help keep column names the same across the file. 

Starts with ```COLUMNS```, followed by the following:

Regular: ```type ColName```

Foreign keys: ```type<ForeignDB::ForeignCol> ColName```

Localized strings: ```locstring ColName``` (see [this](https://wowdev.wiki/Common_Types#langstringref) and [this](https://wowdev.wiki/Localization) page on Wiki, same as"string" type as of 4.0+ but still localized in locale specific files)

Format currently also supports comments by adding ```// Comment goes here ``` at the end of the column definition line, but this is still up for debate.

Valid types that parsers should support: ```int/string/float/locstring```

Unverified columns (guessed, etc) have a ```?``` at the end of ```ColName```.

### Version definitions

```BUILD``` is required. ```LAYOUT``` is required for versions that have it. Can be both.

#### LAYOUT
Line starts with ```LAYOUT``` followed by a list of layouthashes separated by a comma and a space. Can appear only once.

#### BUILD
Line starts with ```BUILD``` followed by a range, multiple exact builds separated by a comma and a space or a single exact build. Can appear multiple times.

#### COMMENT
Line starts with ```COMMENT```, only for humans. Can appear only once.

##### Ranges
```BUILD 7.2.0.23436-7.2.0.23514```. 

Ranges should be specified per minor version to not conflict with other branches. Example:
```
BUILD 7.2.0.23436-7.2.0.23514
BUILD 7.1.5.23038-7.1.5.23420
BUILD 7.1.0.22578-7.1.0.22996
BUILD 7.0.3.21846-7.0.3.22747
```

##### Multiple exact builds 
```BUILD 0.7.0.3694, 0.7.1.3702, 0.7.6.3712```

##### Single build
```BUILD 0.9.1.3810```

#### Columns
```ColName``` refers to exactly the same name as specified in the column definitions. 

No size (floats, (loc)strings, non-inline IDs): ```ColName```

Size (8, 16, 32 or 64, prefixed by ```u``` if unsigned): ```ColName<Size>```

Array: ```ColName[Length]```

Both: ```ColName<Size>[Length]```

With comment (for humans): ```ColName // This is a comment, yo!```

#### Column annotations

```ColName``` can be prefixed with annotations to indicate that this is a special kind of column in this version.

Annotations start with a ```$``` and end with a ```$``` and are comma separated when there's more than one. Current annotations:

**id** this column is a primary key. Example: (inline) ```$id$ColName``` (non-inline) ```$noninline,id$```

**relation** this column is a relationship. Has ```noninline``` when stored in relationship table. Examples: (inline) ```$relation$ColName``` (non-inline) ```$noninline,relation$```

**noninline** this column is **non-inline** (currently only used for  ```$id$``` and ```$relation$```). See non-inline examples above.

## File handling
Files will be saved with DBName.dbd filenames. Every file has multiple definitions for each different structure that has been encountered for that file. Version structures are separated by an empty new line. All line endings should be in Unix format (\n).

## Example definition file
You can view a sample definition [here](https://github.com/Marlamin/WoWDBDefs/blob/master/definitions/Map.dbd).

Discussions regarding the format proposal should be done in the relevant issue [here](https://github.com/Marlamin/WoWDBDefs/issues/1).

All feedback is welcome!
