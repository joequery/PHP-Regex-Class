The class is written so that you can easily add your own regular expression patterns.

It's important to note that the Regex class consists of static methods. That means you do not need to create an instance of the Regex class to use the methods.

The class contains 4 methods as part of the main interface:

Regex::is($type,$value)
Description: returns boolean true if $value is an exact match with the pattern associated with $type. False if otherwise.
Examples:

<?php
echo Regex::is("email","hi@vertstudios.com")? "Found" : "Not Found"; //Found
echo Regex::is("int","A 100 dollar bill")? "Found" : "Not Found"; //Not Found
echo Regex::is("usphone","903-920-9514")? "Found" : "Not Found"; //Found
echo Regex::is("zip","75701")? "Found" : "Not Found"; //Found
echo Regex::is("float",57.10)? "Found" : "Not Found"; //Found
?>

Regex::has($type,$value)
Description: returns boolean true if $value contains at least one instance of the pattern associated with $type. False if otherwise.
Examples:

<?php
echo Regex::has("email","our email: hi@vertstudios.com")? "Found" : "Not Found"; //Found
echo Regex::has("int","A 100 dollar bill")? "Found" : "Not Found"; //Found
echo Regex::has("html","Jolly Jolly")? "Found" : "Not Found"; //Not Found
echo Regex::has("name","#@#$! Arthur")? "Found" : "Not Found"; //Found
echo Regex::has("float","I'm tired. Aren't you?")? "Found" : "Not Found"; //Not Found
?>

Regex::hasAny($types,$value)
$types is a string with various pattern types separated by a comma. Ex: “html,bbcode,int”

Description: returns boolean true if $value contains at least one instance of any pattern associated with $types. False if otherwise.
Examples:

<?php
echo Regex::hasAny("email,usphone","our email: hi@vertstudios.com")? "Found" : "Not Found"; //Found
echo Regex::hasAny("html,bbcode",'<a href="lolspam.html">oeutaoeuaoeu</a>')? "Found" : "Not Found"; //Found
echo Regex::hasAny("html,int,float","Jolly Jolly")? "Found" : "Not Found"; //Not Found
?>

(Note the convenient spam validation of a textarea form submission using Regex::hasAny(“html,bbcode,mailstrings”, $textarea) )

Regex::getDescription($type)
Description: returns a string describing $type

You'll see in the source code that there are two private arrays. $pattern takes the form “type” => “regexpattern”, while $description takes the form “type” => “description”. $description is where these strings are pulled from.***

If you use this class for form processing, it will be useful to get a nicely formatted description of the pattern type.
Example:

<?php
$myform = array("name" => "Joseph", "email" => "Joseph@@vertstudios.com",
                 "usphone" => "9033305057", "zip" => "75AA8");
 
//Iterate through the "form" and output which values are valid.
foreach($myform as $type => $value)
{
    echo Regex::getDescription($type) . " is " .
        (Regex::is($type,$value) ? "valid" : "invalid") . "<br />";
}
//Output:
//Name is valid
//Email is invalid
//Phone is valid
//Zip Code is invalid
?>
(***One could argue that it would make more sense to have the $pattern and $description arrays combined into a multidimensional array, but I felt that it was simply easier to keep the arrays separate and update the two arrays as needed. )