---
title: "Write-Rainbow"
date: 2023-08-29T17:34:30-04:00
categories:
  - blog
  - functions
  - scripts
  - fun
tags:
  - functions
  - color
  - styling
  - fun
author: cptcmdlet
---


# Sometimes you need a little color.

This is something I'm sure exists in a myriad of formats, but here's my take. Write-Rainbows is a PowerShell function that does a write-host but outputs each letter of your string in a different color. Example:

![write rainbows example](/images/writerainbowexample.png)

## The Function 

Here's the function:
`
function Write-Rainbow {
    param(
        [string] $text
    )
    $characters = $text.ToCharArray()
    $charCount = $characters.count
    $charCount = $charCount-1
    $index = 0
    $colors = "red","yellow","green","cyan","Magenta","Blue"
    foreach($character in $characters) {
        if($index -eq $charCount) {
        $color = Get-Random -InputObject $colors
        Write-Host "$character" -ForegroundColor $color
        }
        else{
        $color = Get-Random -InputObject $colors
        Write-Host "$character" -NoNewline -ForegroundColor $color
        $index++
        }
    }
}

`
If you're savvy and just needed the code to figure that out, I guess you can go! 

## Bit by bit

Now that we have the code and the fancy pants people have left, let's go section by section. 

### Defining the function

I want to be able to use this over and over again so I am writing it as a function. 

`function Write-Rainbow {
    param(
        [string] $text
    )`

"function" is probably obvious, that's what we're creating and that's where we're telling PowerShell what we are up to. Write-Rainbow is the function name, it's what we'll type to execute this block of code whenever we want. Defining a parameter let's the function take input from the user that alters the way the script or function will run, that's what we're doing with Param. The [string] setting tells PS we're expecting a string datatype, and $text is the variable where the user input is stored for utilization by the function. 

### Put the characters into an array and count them.

This whole rainbow writer thing works by doing a write-host for each individual letter of the string. To accomplish this we put the string into an array.

`$characters = $text.ToCharArray()`

The .NET framework makes this nice and easy for us by including a .toCharArray() method. If you're new to PowerShell, don't be intimidated by this. Just know that one of PowerShell's strengths is how well it interacts with .NET and that this particular method will take the characters in a string and put them into an array. They created this "method" for likely the very same reason we want to put Write-Rainbow as a recallable function. I don't want to reinvent the wheel every time I want to add some color to my scripts. Neat-o.

Let's see what happens if I look at the $characters array directly with our example:

![toCharArray example](/images/chararray.png)

`$charCount = $characters.count`

We're going to need to know when to stop, so I've counted the $characters in our array.

`$charCount = $charCount-1`

I want to do something different for the last character of each string we're rainbow-izing so reducing the count by one will help. 

`$index = 0`

This is what we'll increment to keep track of where we are in the string.

### Define color choices

Let's definite our color options in an array.

`$colors = "red","yellow","green","cyan","Magenta","Blue"`

### Loop de loop

I love a foreach loop in the morning. Recall that this works by doing a write-host for each individual letter in our string. 

`foreach($character in $characters) {`

Here we tell PowerShell "for each character in our characters array do this" the "this" comes after the bracket. The syntax will deduce what you mean so if you want to say foreach($char in characters), or ($crs in $characters), or whatever makes sense to you - you may. 

### If...

We set an $index to increment and alter the script logic, we start by checking if that logic condition is met:

`if($index -eq $charCount) {`

When using write-host you can keep your results on the same line utilizing the -noNewLine, which we'll put to use, but for the last character of the string we want the line to end and the result to print. That's what we're doing here after we choose a random color from our pool of options:

`$color = Get-Random -InputObject $colors`

Get-Random is a built in PowerShell command that gets a random value, the -InputObject parameter tells Get-Random from where to choose its values, in this case we've defined our $colors array.

Once we have a color, we can write-host the final character.

`Write-Host "$character" -ForegroundColor $color`

### Else...

If this isn't the final character of the string we are rainbow-ifying, we want to do something else, we want continue printing characters on the same line. 

`else{
        $color = Get-Random -InputObject $colors
        Write-Host "$character" -NoNewline -ForegroundColor $color
        $index++
        }`

You should recognize the $color selection, and write-host has the -nonewline option set. The new piece in this bit is the $index++, that's where we increment the $index value we check to keep track of whether or not we are dealing with the end of our string.` ++ `increments the value by one, it's an easy shorthand. Yep, `--` works to subtract, if you were wondering!

# You have been rainbowified.

There you have it. All the pieces of the puzzle. Go forth and be colorful. 


# References 

Here's help articles for the concepts utilized in this script:

[About Functions](https://learn.microsoft.com/en-us/powershell/scripting/learn/ps101/09-functions?view=powershell-7.3)

[About Arrays](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_arrays?view=powershell-7.3)

[About String.toCharArray](https://learn.microsoft.com/en-us/dotnet/api/system.string.tochararray?view=net-7.0)

[About Methods](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/methods)

[About Foreach Loops](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_foreach?view=powershell-7.3)

[About If, else](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_if?view=powershell-7.3)

[About Get-Random](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-random?view=powershell-7.3)

[About Operators](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_operators?view=powershell-7.3)


