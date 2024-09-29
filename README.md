# MusicString

Single line music notation syntax. See *musicstring.md* for syntax definition*.

Project was started by Marcus Gustafsson in 2019. It is a formalization and extension based on a system that he has used and developed since 2011, for quick notation of melodies on-the-go and for his music theory teaching.

Syntax is in some areas influenced by [ABC notation](https://abcnotation.com/).

## Simple example

```musicstring
1 2 3h
```

... would parse as two quarter notes of c and d, followed by a halv note of e, while...

```musicstring
@CfKDM3O3 12 3456 7_87 65_4 3.2 1
```

... would give a d major scale (f clef, 3/4 meter starting in octave 3) with two eights, four sexteens, eight+two sexteens, two sexteens+eight, dotted eight+sixteen and a quarter.
