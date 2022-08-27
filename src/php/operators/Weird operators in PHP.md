# Weird operators in PHP

- [Weird operators in PHP](#weird-operators-in-php)
  - [1. `b`’ operator](#1-b-operator)
  - [2. []= operator](#2--operator)
  - [3. On the left object operator <-](#3-on-the-left-object-operator--)
  - [4. ––> operator](#4--operator)
  - [5. Scope Resolution Operator (::)](#5-scope-resolution-operator-)
  - [6. <= : left arrow for arrays [ $a <= $b ]](#6---left-arrow-for-arrays--a--b-)
  - [7. Constant names with *, %](#7-constant-names-with--)
  - [8. Battleship operator](#8-battleship-operator)
  - [9. tiny spaceship](#9-tiny-spaceship)
  - [10. X-fighters](#10-x-fighters)
  - [11. battleship operator](#11-battleship-operator)

## 1. `b`’ operator

Simply add a b before any string, and it will do nothing. This neat trick works with b and B and nothing else.

```php
$string = b'content';
```

## 2. []= operator

The short array append operator. It works just as you expect, by pushing the element on the right side to the array on the left.

```php
$array []= 'element';
```

It is a lot more elegant than `$array[] = 'element'`;

## 3. On the left object operator <-

The left object operator was introduced in PHP to extend the ways of writing code : coders could write PHP right to left. Although the project was later discontinued for obscure reasons, the first operator to be supported was left in PHP.

```php
$c = $a>-B;
```

For backward compatibility purposes, this code doesn’t reach the property B in the object $a, but simply compares $a to the opposite of B.

## 4. ––> operator

> --> is also known as the ‘super object operator’, built on top of its remote cousin, ->.

```php
$object-->property
 ```

## 5. Scope Resolution Operator (::)

> The Scope Resolution Operator (also called Paamayim Nekudotayim) or in simpler terms, the double colon, is a token that allows access to static, constant, and overridden properties or methods of a class.

When referencing these items from outside the class definition, use the name of the class. It's possible to reference the class using a variable. The variable's value can not be a keyword (e.g. self, parent and static)

```php
<?php
class MyClass {
    const CONST_VALUE = 'A constant value';
}
$classname = 'MyClass';
echo $classname::CONST_VALUE;
echo MyClass::CONST_VALUE;
?>
```

## 6. <= : left arrow for arrays [ $a <= $b ]

> We all know the => operator for the arrays, but arrays also support the mirror version, like this :

```php
$array = ['a'  => 'b', 
          'c' <= 'd', ];
```

Do some mirror-writing. This operator also has the ability to turn both operands into a boolean, for very compact storage.

## 7. Constant names with *, %

Constants are nice and efficient, until you want more freedom for their syntax. Although the manual states that constants should only have letters, underscores and figures, it is also authorized to use special characters like * or % in the name. Just like this :

```php
$x = foo( A*B );
```

The only preparation you have to do is to make sure that constants A and B exist, and that their product has the value of your intended constant. This is another good reason to avoid prime numbers as constant values

## 8. Battleship operator

So, since PHP 7 introduced it, everyone know about the spaceship operator <=> . It is less know that a whole fleet of spaceship was introduced at the same time, to offer large fleet scale operations.

## 9. tiny spaceship

You may provide an escort to the spaceship operator, by using the tiny spaceship operator. This operator is not a comparison : it simply adds $b to $a, with a lot of style

```php
$a -=- $b;
```

## 10. X-fighters

In case you want to add some firepower to the previous fleet, you can summon X-fighters to the PHP source : +-0-+. The following code adds 3 to $a

```php
$a = $a +-0-+ 3;
```

## 11. battleship operator

The battleship operator provides support for the previous operators. It is one of the ‘array only’ operators, just like =>. It should only be used by the most advanced PHP gurus ever on this side of the galaxy. Do not underestimate its power.

```php
$x = [ 2 <=['-']=> $b];
```

Isn’t that a much better way to write this ?

```php
$x = [ 1 => $b];
```
