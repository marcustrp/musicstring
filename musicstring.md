# MusicString

## 1. About

Single line music notation with some inspiration from ABC notation. Originally made to quickly create music theory exercises and quickly writing down melodies on-the-go.

This repo is only a specification of the MusicString syntax. For an implementation, see MusiCore.

### Version

This is the 0.1.0 work-in-progress version of the specification. **Breaking changes may - or rather WILL - occur.**

### Overview

Allows for quick creation of short music examples in a single line of plain ASCII.

#### Example 1: Rhythm without pitch

`M2 x_xx xx xx_x xx.`
(todo: picture)

#### Example 2: Rhythm without pitch, triplets

`xx 3:xxx 3::4:x_x_xx 3::2 xx_`
(todo: picture)

#### Example 3: Simple melody in C major

`1 5 33 1 22 -55 8`
(todo: picture)

#### Example 4: Melody in G and 3/4

`@KGM3 5+1 -7_67 +1 2.3 4_32 3 -6.7 +13 21 -7.6 5#4 5`
(todo: picture)

## 2. Specification

````regexp
[Extensions] [Information] [Modifiers] [Group]
````

### Extensions

Extension fields are prefixed with and & symbol. Can be used for adding extra information, for example for defining exercise settings (see MusicStringX).

### Information fields

Information fields are prefixed with @ symbol. Multiple information fields in the same declaration are allowed, like @CgM3O4

Fields marked "header only" may only be defined at the start of musicstring.

#### Book 'B' (header only)

``B`[book]` ``

Book must be enclosed with backticks. May be used multiple times to add more books.

#### Clef 'C'

````regexp
C[gfatsmpn][0-5]?([+-]8|15)?
````

Set clef to use. Default clef is g (treble).

Optional number is clef line (counting from below) or (perc and none), number of staff lines to display.

By adding +8, -8, +15 or -15, octave clefs are supported. For example, Cg-8 adds an 8 below treble staff.
@todo is octave clefs taken into account for note input or not?

| Token | Clef | Notes |
| ----- | ----------- | ----- |
| `f` | bass | same as f4 |
| `g` | treble | same as g2 (default clef) |
| `t` | tenor | same as c4 |
| `a` | alto | same as c3 |
| `m` | mezzosoprano | same as c2 |
| `s` | soprano | same as, c1 |
| `p` | percussion | (p0 = no lines, p1 one line and so one) |
| `n` | none | number work as for percussion |

#### Discography 'D' (header only)

``D`[discography]` ``

Discography must be enclosed with backticks. May be used multiple times to add more items.

#### Composer 'E' (header only)

``E`[composer]` ``

Composer must be enclosed with backticks. May be used multiple times to add more creators.

#### Input scale 'I'

````regexp
I[mode]
````

@deprecated With new key syntax, this will probably be removed.

Set scale used for note numbers in body (two letter code). Default depends on key, see **Key** for details. Note that setting the key will also update input scale.

Numbers in examples below shows what using 1234567 (se **Body**) is in selected input scale.

| Token | Mode | Scale steps |
| ----- | ----------- | ----- |
| `ma` | major | 1 2 3 4 5 6 7 |
| `mi` | minor | 1 2 b3 4 5 b6 b7 |
| `hm` | harmonic minor | 1 2 b3 4 5 b6 7 |
| `mm` | melodic minor | 1 2 b3 4 5 6 7 |
| ----- | ----------- | ----- |
| `io` | ionian | *same as major* |
| `do` | dorian | 1 2 b3 4 5 6 b7 |
| `ph` | phrygian | 1 b2 b3 4 5 b6 b7 |
| `ly` | lydian | 1 2 3 #4 5 6 7 |
| `mx` | mixolydian | 1 2 3 4 5 6 b7 |
| `ae` | aeolian | *same as minor* |
| `lo` | locian | 1 b2 b3 4 b5 b6 b7 |

#### Key 'K'

````regexp
K[root][mode][inputscale]
````

@todo Cleanup, remove prior versions if new is stable.

Set key. Default key is C major.

##### Key: Prior version

Set key. Default key i C major. Setting the key will also set *input scale*.

##### Root and major/minor

Root note (a-g or A-G) with optional sign (b or #). Uppercase A-G is major and lowercase a-g is minor. For no key, use 'n'. When using no key, notes are related to c.

###### Root: Prior version

Root note (a-g,  A-G or n) with optional sign (b or #). If mode is omitted, uppercase A-G is major and lowercase a-g is minor.

(notes for dev)

- root
  - define root note
  - also major/minor?
- mode
  - affects key signature
  - BUT: the mix with scales (harmonic, melodic, blues, penta) is strange in this context, because those do NOT normally affect key signature (allthough it could be useful for music theory purposes?)
  - is there any use of scale? It's currently in Score.scale, but it seems strange to have one scale for a score. For music theory exercises it could be useful to define a scale in a musicstring, though
- input scale
  - affects import of musicstring, but nothing else

##### Mode (should this be scale?)

One letter code for mode. If no mode is defined, mode is set to major or minor (aeolian) using lower- or uppercase letter for root.

| Token | Scale | Notes |
| ----- | ----------- | ----- |
| `h` | harmonic | (requires root to be lower case, otherwise ignored) |
| `m` | melodic | (requires root to be lower case, otherwise ignored) |
| ----- | ----------- | ----- |
| `i` | ionian | (same as major) |
| `d` | dorian | (input scale: dorian) |
| `y` | phrygian | (input scale: phrygian) |
| `l` | lydian | (input scale: lydian) |
| `x` | mixolydian | (input scale: mixolydian) |
| `a` | aeolian | (same as minor) |
| `c` | locrian | (input scale: locrian) |
| ----- | ----------- | ----- |
| `p` | pentatonic | (input scale: major) |
| `b` | blues | (input scale: minor) |
| ----- | ----------- | ----- |
| `?` | no mode change | use major/minor as defined by root |

###### Mode: Prior version

Two letter pair for mode. Some modes set different key and scale. Also see **input scale** Available modes are:

- ma major (input scale: major) See: root
- mi minor (input scale: minor) See: root
- hm harmonic minor (scale, key is minor) (input scale: minor)
- mm melodic minor (scale, key is minor) (input scale: minor)

- io ionian (same as major)
- do dorian (input scale: dorian)
- ph phrygian (input scale: phrygian)
- ly lydian (input scale: lydian)
- mx mixolydian (input scale: mixolydian)
- ae aeolian (same as minor)
- lo locrian (input scale: locrian)

- mp major pentatonic (input scale: major)
- ip minor pentatonic (input scale: minor)
- mb blues (using major key, blues scale) (input scale: minor)
- ib blues (using minor key, blues scale) (input scale: minor)

- no no key (input scale: major)

##### Input scale

Set scale used for note numbers in body (two letter code). The default is major for all keys and modes, but it can be overridden by using one of these one letter codes.

Numbers in examples below shows what using 1234567 (se **Body**) is in selected input scale.

| Token | Scale | Scale steps |
| ----- | ----------- | ----- |
| `h` | harmonic | minor 1 2 b3 4 5 b6 7 |
| `l` | melodic | minor 1 2 b3 4 5 6 7 |
| `i` | ionian | (same as major) |
| `d` | dorian | 1 2 b3 4 5 6 b7 |
| `p` | phrygian | 1 b2 b3 4 5 b6 b7 |
| `l` | lydian | 1 2 3 #4 5 6 7 |
| `x` | mixolydian | 1 2 3 4 5 6 b7 |
| `a` | aeolian | (same as minor) |
| `c` | locrian | 1 b2 b3 4 b5 b6 b7 |

#### Meter 'M'

````regexp
M[Symbol|Numeric]
````

Default meter is 4/4.

##### Symbols

Supported symbols are 'c' (common time) and 't' (cut time).

##### Numeric

Standard format is numerator/denomenator, like 3/4, 9/8, 12/8 and so on.

**Short mode 1**
One digit is shortcut for x/4. M3 = M3/4, M2 = M2/4 and so on.

**Short mode 2**
Two digits is short for x/y, so M28 is M2/8.

#### Notes 'N' (header only)

``N`[notes]` ``

Notes (comments) must be enclosed with backticks. New lines should be replaced with \n.

#### Octave 'O'

````regexp
O[0-9]
````

@todo Define default octave for different clefs.

Sets the current octave

#### Origin 'o' (header only)

``o`[origin]` ``

Must be enclosed with backticks.

#### Tempo 'Q'

``Q`[Tempo]` ``

@todo Define syntax for tempo (`Allegro`, q=120, 'rit'...)

#### Type 'R'

``R`[Type]` ``

Must be enclosed with backticks. Reel, waltz...

#### Source 'S'

``Q`[Source]` ``

Must be enclosed with backticks.

#### Title 'T' (header only)

``T`[title]` ``

Title must be enclosed with backticks.

#### Transcription 'Z'

``Z`[Transcription]`

Must be enclosed with backticks.

### 2.3 Modifiers

**NOTE: Octave modifiers removed as of 16/10 2023 (octave change in body changes octave). Currently, no modifiers are available.**
~~Available modifiers:~~

| Modifier | Description |
| ----- | ----------- |
| + | ~~Increases current octave by one~~ |
| - | ~~Decreases current octave by one~~ |

### Barlines, repeats and endigs

^(!(?!(?:!|!!)).*!)*(?:([|\.\|\[:\\]{1,10}[0-9-,]*)$|([\[\]|:\.\\]*))$
~~^(!(?!(?:!|!!)).*!)*(:)*(\.)?([\|]*[\]]?)?(\\)?([\[]?[\|]?)?(:)*([1-9,-]*)?~~

@todo Add support for line break using barline

Single barlines are not required as they are automatically added.

| Token | Description |
| ----- | ----------- |
| `!fermata!` | Decorations |
| `:` | Repeat end |
| `.` | Dotted barline |
| `|]` | Barline type (end of bar) |
| `\\` | Line break |
| `[|` | Barline type (start of next bar) |
| `:` | Repeat start |
| `]` | Used with hidden barline |
| `1,2-3` | Ending number(s) |

#### Barline types

| Token | Description |
| ----- | ----------- |
| `|` | Single barline |
| `||` | Double barline (light-light) |
| `|]` | light-heavy |
| `[|` | heavy-light |
| `.|` | dotted barline |
| `[|]` | hidden barline |

#### Repeats and endings

| Token | Description |
| ----- | ----------- |
| `:|` | Repeat end with light-heavy barline, :|] not required |
| `|:` | Start repeat with heavy-light barline, [|: not required |
| `::` | Repeat end and start, light-heavy-light barline, :|: or other variants not  |required
| `[1` | First ending |
| `[2-3` | Second and third ending |
| `[1,3` | First and third ending |
| `:|[2` | Second ending |

#### Line break

| Token | Description |
| ----- | ----------- |
| `\` | Line break, single barline (alternative syntax: \| ) |
| `||\` | Double barline, then line break |
| `\|:` | Single barline, then line break and start repeat |
| `:|\[2` | Repeat end, then line break and second ending |

### Body

| Token | Description |
| ----- | ----------- |
| `(` | Slur start |
| `3:2:4:` | Duplets, triplets etc. |
| `!f!!fermata!` | Decorations |
| ``IVm6`` | Step (analysis) |
| `"Cmaj7/E"` | Chord symbol |
| `'Hel-'` | Lyrics |
| `´Do´` | Solmization |
| `*D64*` | Function (analysis) |
| `[` | *Start of group* |
| `+-/` | Octave shift |
| `bb|b|#|x|m` | Accidental |
| `1-9rxyi` | Note number or rest/non-pitched/spacer/invicible |
| `IL` | Note property: locked |
| `]` | *End of group* |
| `\_\_|[ldwhqestuv_]` | Length |
| `. | Dot(s) |
| `=` | Tie |
| `)` | Slur end |

#### Slur (start/end)

A '(' indicates start of a slur and ')' the end of a slur. Slurs may start and stop on different body items. Nested slurs are allowed, as are multiple slur starts/endings on the same note.

#### Duplets, triplets etc

*This section is heavily based on abcnotation.net.*

**NOTE:** Only fully qualified definitions implemented (p:q:r:) except for 3: which defaults to 3:2:3

Triplet definition must always end with a colon. Format p:q:r: , where "put p notes into the time of q for the next r notes". Defaults for q and r are as follows:

| Token | Description |
| ----- | ----------- |
| `2:` | 2 notes in the time of 3 |
| `3:` | 3 notes in the time of 2 |
| `4:` | 4 notes in the time of 3 |
| `5:` | 5 notes in the time of n |
| `6:` | 6 notes in the time of 2 |
| `7:` | 7 notes in the time of n |
| `8:` | 8 notes in the time of 3 |
| `9:` | 9 notes in the time of n |

If the time signature is compound (6/8, 9/8, 12/8) then n is three, otherwise n is two.

If q is not given, it defaults as above. If r is not given, it defaults to p.

NOT IMPLEMENTED: For the common eight note triplets, there is a simplified syntax: 123 is equal to 3:123.

#### Decorations

@todo Body decorations (see also barline)

#### Step (analysis)

Step (analysis) are enclosed within back ticks.

@todo: Specify notation of step, like inversions, secondary dominants...

#### Chord symbol

Chord symbols are enclosed within double quotation marks.

@todo Specify syntax, like use of parenthetis...
@todo Allow different levels (lines)
@todo Check if offset chordsymbols are supported

#### Lyrics

Lyrics are enclosed within single quotation marks.

@todo Specify syntax, like using -

#### Solmisation

Solmisation is enclosed within forward ticks.

#### Function (analysis)

Function analysis are enclosed between asterixes.

@todo Specify syntax, like bass, key change with support for multiple rows, D4-3 between notes

#### Octave shift

A + or - is used to change the current octave.

~~Single notes may be prefixed with + or - to increase or decrease current octave. These are per note only (see **Modifiers**) *NOTE: as of 17/10 2023 octave shifts changes tune octave, not for single note.*~~

A single note can be octave shifted down with /. Digits 8 and 9 can be used to avoid shifting octave up for 1 (8) and 2 (9).

#### Accidental

Accidentals are for the *note number, not written accidentals*. They depend on the currect **input scale** being used. If input scale is minor (and therefore 3 is translated to b3), use m3 to use "3 as in major".

**CHANGE PROPOSSL** *Make all scaleNumbers relate to major, requiring minor to use b3, b6 and b7. It would be more consistent, and would also remove the need for the strange 'm' accidental. REPLY: No, writing minor without b3/b6/b7 is really convenient.*

#### Note number or rest

Note numbers are from 1-9, where 8 and 9 can be used to avoid changeing octave (for example, 5897 instead of 5+12-7). For rest, use r.

Unpitched rhythms can be written with x instead of 1-9, like x_xx.

TODO: document spacer 'y'.

Use 'i' to create an invisible note. Useful when making theory exercises for hiding single notes. If an 'i' is used in a chord (like [135i]), all notes will be marked as invisible. Also, see "Note properties" 'I'.

#### Note properties

`IL`

Use 'I' to mark note as invicible (useful if hiding an answer in an exercise)

Use 'L' to mark note as locked (editors can use this to disable editing of specific notes)

#### Note groups

@todo document note groups

See also: Duplets, triplets etc.

## 3. Complete RegExp

*Code below might be outdated, was previously used for referece.*

### Information regexp

```regexp
^(?:C([gfatsmpn]))?(?:K([abcdefg][#b]?)([a-z]{2}))?(?:M([ct1234567890]{1,2})(?:\/([1248636]{1,2})))?(?:O([0-9]))?
```

### Bar regexp

````regexp
^(!(?!(?:!|!!)).*!)*(?:([|\.\|\[:]{1,10}[0-9-,]*)$|([\[\]|:\.]*))(br)?$
````

### Modifiers

@todo document

### Body regexp

in regexp below, replace {NAME} with code below...

{OCTSIGN}
changes

```regexp
  (?:[+-]){0,2}(?:[bb|b|#|x|](?!x))?
```

// length {LENGTH}
Changes

- removed |8|16|32|64|128| to enable groups
- added e (8), s (16), t (32), u (64), v (128)
- added \_ and \_\_, used in groups and also as an option for single notes (1 is q, 1_ is h, 1__ is w )
  // OLD [ldwhqest]|8|16|32|64|128|**|\_

```regexp
  __|[ldwhqestuv_]
```

// number {PITCH}
Changes

- added x to use for unpitched rhythms
- support for groups: xx in 4/4 beams and make notes two eight notes, x_xx is 8 16 16, x.x is 8. 16
  // [1-7xy]
  [1-7rxy]

{PREFIX}

```regexp
([\(]*)?
(!(?!(?:!|!!))._!)_
(?:\`([^\"]_)\`)?
(?:\"([^\"]_)\")?
(?:'([^']_)')?
(?:´([^']_)´)?
(?:\*([^\*]\*)\*)?
```

{POSTFIX}
Changes

- rest removed (), ([r])\*
- tie changed from > to = ()
  - and before that to > from - (clashed with octave change -)

```regexp
  ([\.]\*)
  (>)?
  ([\)]*)?
```

---

```regexp
{PREFIX}
(?:
// length
({LENGTH})
|
// chord and length
(?:\[((?:{OCTSIGN}{PITCH}){0,32})\]({LENGTH}))
|
// chord
(?:\[((?:{OCTSIGN}{PITCH}){0,32})\])
|
// number and length
(?:({OCTSIGN}{PITCH})({LENGTH}))
|
// number
({OCTSIGN}{PITCH})
)
{POSTFIX}
```
