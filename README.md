#Fast generation OpenDocument reports
<table>
    <tr>
        <td>![](https://github.com/xv1t/OpenDocumentTemplate/blob/master/docs/img/document_template_src.jpg)
        <td>![](https://github.com/xv1t/OpenDocumentTemplate/blob/master/docs/img/document_template_out.jpg)
    </tr>
    <tr>
        <td>![](https://github.com/xv1t/OpenDocumentTemplate/blob/master/docs/img/continents_template_src.jpg)
        <td>![](https://github.com/xv1t/OpenDocumentTemplate/blob/master/docs/img/continents_template_out.jpg)
    </tr>
</table>

Recommended software for create template
* LibreOffice
* OpenOffice

Your done report files was correct opened in
* LibreOffice
* OpenOffice
* MS Office >=2010

## Fast manual
1. Create template ods file
2. Put elements
3. Mark ranges
4. Load data from database
5. Render data through template file info your new ods file
6. Open in the LibreOffice Calc or other and enjoy


## Requirements
Php extensions
* zip
* xml

Php version >=5.3

Recommended sowfware for templating: LibreOffice 5

##Install
Put file `OpenDocumentTemplate.php` into your project, and use it
```php
<?php
include_once "OpenDocumentTemplate.php";

//create object

$template = new OpenDocumentTemplate();
```

#First simple report

## Prepare your data
Data - is array in php. Recommend all object fields group in object name.



```php
<?php
$data = array(
    'Report' => array(
        'name' => 'Test Report',
        'date' => '2016-09-25',
        'author' => 'Me'
    )
);
```
All fields grouping in the `Report` parent array.
This method of naming provides a powerful technique for multidimensional data.

## Design template file
Open the LibreOffice Calc. Create new spreedsheet;

add a 3 cells for next contents:

   |A   | B |   C
---|---|---|----
1 | [Report.name] | [Report.date] | [Report.author]
2 | | |
3 | | |

Save it with name `sample_report.ods`.

## Render template with data
```php
<?php
$template->open('sample_report.ods', 'sample_report-out.ods', $data)
```
And open new file `sample_report-out.ods` and you see in sheet:

  |A   | B |   C
---|---|---|----
1 |Test Report | 2016-09-25 | Me
2 | | |
3 | | |

## Add second dimension
Add a key `Cities` for the list of objects
```php
<?php
$data = array(
    'Report' => array(/* main Report data */),
    'Cities' => array(
        array(/* city data */),
        array(/* city data */),
        array(/* city data */),
        array(/* city data */),
    )
);
```

All `Cities` object must be have an identical list of the fields. In our example: `name`, `streets`, `population`
```php
<?php

//Sample of one City object
array(
    'City' => array(
        'name' => 'Albatros',
        'streets' => 165,
        'population' => 1300000
    )
);
```

And  all data
```php
$data = array(
    'Report' => array(
        'name' => 'Test Report',
        'date' => '2016-09-25',
        'author' => 'Me'
    ),
    'Cities' => array(
        array( //first object
            'City' => array(
                'name' => 'Albatros',
                'streets' => 165,
                'population' => 1300000
            )
        ),
        array( //next object
            'City' => array(
                'name' => 'Turtuga',
                'streets' => 132,
                'population' => 750000
            )
        ),
        array( //next object
            'City' => array(
                'name' => 'Palmtown',
                'streets' => 18,
                'population' => 10000
            )
        ),
    )
);
```
## Add a other object as linear dimesion 
Add inforamation about the mayor of the each city, in the sibling key `Mayor`
```php
<?php

$data = array(
    'Report' => array(/*...*/),
    'Cities' => array(
        array(
            'City' => array(/*...*/),
            'Mayor' => array(
                'name' => 'John Do',
                'old' => 47
            ),
        ),
        array(
            'City' => array(/*...*/),
            'Mayor' => array(
                'name' => 'Mary Ann',
                'old' => 32
            ),
        ),
        array(
            'City' => array(/*...*/),
            'Mayor' => array(
                'name' => 'Mike Tee',
                'old' => 29
            ),
        ),
    )
);
```

## Add third dimesions
Add a key `Squares` for the list of the squares in the each city

```php
<?php
$data = array(
    'Report' => array(/*...*/),
    'Cities' => array(
        array(
            'City'  => array(/*...*/),
            'Mayor' => array(/*...*/),
            'Squares' => array(
                array(/*...*/),
                array(/*...*/),
                array(/*...*/),
            )
        ),
        array(
            'City'  => array(/*...*/),
            'Mayor' => array(/*...*/),
            'Squares' => array(
                array(/*...*/),
                array(/*...*/),
                array(/*...*/),
                array(/*...*/),
                array(/*...*/),
            )
        ),
        array(
            'City'  => array(/*...*/),
            'Mayor' => array(/*...*/),
            'Squares' => array(
                array(/*...*/),
                array(/*...*/),
            )
        ),
       
    )
);
```
Example Square object:
```php
<?php
array(
    'Sqaure' => array(
        'name' => 'Trafalgaar',
        'length' => 23,
        'width' => 45
    )
)
```

And out `$data` finish version:
```php
<?php

$data = array(
    //level1
    'Report' => array(
        'name'   => 'Test Report',
        'date'   => '2016-09-25',
        'author' => 'Me'
    ),
    'Cities' => array(
        array(
            //level 2
            'City' => array(
                'name'       => 'Albatros',
                'streets'    => 165,
                'population' => 1300000
            )
            'Mayor' => array(
                'name' => 'John Do',
                'old'  => 47
            ),
            'Squares' => array(
                array(
                    //level 3
                    'Sqaure' => array(
                        'name'   => 'Trafalgaar',
                        'length' => 23,
                        'width'  => 45
                    )
                ),
                array(
                    'Sqaure' => array(
                        'name'   => 'Square #2',
                        'length' => 23,
                        'width'  => 45
                    )
                ),
                array(
                    'Sqaure' => array(
                        'name'   => 'Square #3',
                        'length' => 23,
                        'width'  => 45
                    )
                ),
            )
        ),
        array(
            'City' => array(
                'name'       => 'Turtuga',
                'streets'    => 132,
                'population' => 750000
            )
            'Mayor' => array(
                'name' => 'Mary Ann',
                'old'  => 32
            ),
            'Squares' => array(
                array(
                    'Sqaure' => array(
                        'name'   => 'Square #4',
                        'length' => 23,
                        'width'  => 45
                    )
                ),
                array(
                    'Sqaure' => array(
                        'name'   => 'Square #5',
                        'length' => 23,
                        'width'  => 45
                    )
                ),
            )
        ),
        array(
            'City' => array(
                'name'       => 'Palmtown',
                'streets'    => 18,
                'population' => 10000            
            ),
            'Mayor' => array(
                'name' => 'Mike Tee',
                'old'  => 29
            ),
            'Squares' => array(
                array(
                    'Sqaure' => array(
                        'name'   => 'Square #6',
                        'length' => 23,
                        'width'  => 45
                    )
                ),
                array(
                    'Sqaure' => array(
                        'name'   => 'Square #7',
                        'length' => 23,
                        'width'  => 45
                    )
                ),
                array(
                    'Sqaure' => array(
                        'name'   => 'Square #8',
                        'length' => 23,
                        'width'  => 45
                    )
                ),
            )
        ),
    )
);
```
## Design a spreedsheet

   | A | B | C |  D | E
---|---|---|---|---|----
1 | [Report.name] | | Author | [Report.author]
2  | **Cities**
3  |   | City | [City.name] 
4  |   | Streets | [City.streets]
5  |   | Population | [City.population]
6  |   | Mayor | [Mayor.name]
7  |   | **Squares** 
8  |   |          | name | length | width
9  |   |           | [Square.name] | [Square.length] | [Square.width]
11 |   | Squares count | [COUNT(Squares)]
12 | Cities count | [COUNT(Cities)]
13 | Report date | [Report.date]



Well, we have a 3 level dimension array of objects:

In LibreOffice Calc they are called `Range names`. 

`Insert` -> `Names` => `Manage`


# Examples


