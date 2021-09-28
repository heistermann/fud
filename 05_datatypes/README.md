# Level 5: Datentypen - ein Blick hinter die Kulissen

![img](https://imgs.xkcd.com/comics/file_extensions.png)

(Quelle: [xkcd](https://xkcd.com/1301/), Creative Commons Attribution-NonCommercial 2.5 License.)


## Python

- Seminarinhalte in Python ([Link](python/Datatypes.html))

## R

- Seminarinhalte in R ([Link](R/Datatypes.html))


## Lernziele

- Grundlegende (primitive) Datentypen: bool, integer, float, string
- Warum müssen wir uns mit so etwas beschäftigen?
- Zusammengesetzte/strukturierte Datentypen: lists, dictionaries, ... 
- Daten und Dateien
- Logische Operationen
- Manipulieren von Zeichenketten

## Inhalte der Coding-Werkstatt
- Habt Ihr offene Fragen zu den Inhalten des Seminars?
- Bearbeitung der Übungsaufgaben ([R](R/exercises05.html), [Python](python/exercises05.html))
- Mögliche Vertiefungsaufgaben:
     - Pegeldaten des Global Data Runoff Center (GRDC) einlesen
     - Webcam-Daten des DWD verarbeiten
     

[comment]: <> ## Struktur

[comment]: <> A data type is a classification that specifies the type of data that a variable or object can hold. There are **primitive data types** which are typically built-in or basic to a programming language, and include Boolean type (true and false) and numeric types (integer and floating point data types). **Composite types** are obtained from more than one primitive type and organized into data structures. For example, an array or vector is a sequence of a number of elements that comprise Boolean or numeric types. Strings and text types also belong to composite types as they store a sequence of characters from a set of available character encoding standard such as ASCII (American Standard Code for Information Interchange).

[comment]: <> There are **data structures** that organize data. For example, tabular data can be stored as a data.frame in R and ... in Python. Tabular data is organized into columns and rows where rows commonly refer to observations and columns to variables or components. Variables may have a different scale, e.g. metric scale, nominal scale or ordinal scale. This scale should be consistent for a variable, i.e. the values stored in a column must have the same data type. At the same time, there should be the same number of observations for each variable so that each column of the table has the same length. Tabular data structures are built in a way so that users supply their data consistently and comprehensively.

[comment]: <> There are data structures that are more advanced. Think of spatial data. Spatial data may consist of vector geometries that are associated with tabular data. Each feature (e.g. a point, line or polygon) is linked to attribute data stored in a row of a table. Moreover, the vector geometries are linked to some coordinate system that locate the data to a place on Earth or any other planetary body. Encapsulating all this information in a data structure makes handling this diverse data easy.

[comment]: <> Finally, data structures may be associated with a set of particular methods. If so, then a data structure is likely an object, and we enter the realm of **Object Oriented Programming** (OOP). An OOP object is a construct that does things - it behaves and acts, if you like. A slider in a graphical user interface can be an object that can set a particular value, and that can react to user input. Another object could be a linear model which contains the type of model, the  parameters and their values, and the data from which it was derived. You can use this object to make predictions, or calculate uncertainties of parameters. To this end, we will learn that R and Python rely on the object-oriented programming paradigm.

[comment]: <> In this session, we will go through the different data types and data structures. We will see that in R, everything is stored as object. And we will learn to do basic operations with these objects such as indexing, concatenation and querying.
