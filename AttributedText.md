# Attributed Text
Attributed text lets you format text to display in a SKLabel

[1] Developer ethanhuant13 created a really nice builder pattern, easy to work with

[2] Developer SteveBarnegren has a pattern that isn't so great, but the examples on the site are great.

When using a NSMutableAttributedString (aka mas) for a SKLabel you have to remember to reassign the attributedText value with each update.  And don't set the SKLabel.text value.

 - [1](https://github.com/ethanhuang13/NSAttributedStringBuilder)
 - [2](https://github.com/SteveBarnegren/AttributedStringBuilder)
