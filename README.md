# ScorePacks2InstrumentPacks
Distributes a set of music scores by copying instrument part files from a song directory to a pre-made instrument pack. Useful for managing and distributing an orchestra's sheet music to its musicians.

## Command line options:

 ```-h```             Displays this page
 
 ```-s```             Source directory, where all the sheet music is held.
                Either specify a specific song directory,
                e.g. 'Sheet Music/Blues for a Billious Bystander', 
                in which case only this song's instruments are distributed.
                or specify the root of all the song directories, 
                e.g. 'Sheet Music'
                in which case all the songs in the directory are distributed.
                If not specified, then the current working directory is used.
                
 ```-d```             Destination directory, populated with instrument directories.                

Typical use:
```
+-Sheet Music
:  +...
|
+-Instrument Packs
:  +...
```

If the Current Working Directory is 'Sheet Music', then to process all the files in it, do this:
```
./ScorePacks2InstrumentPacks.sh -d ../Instrument\ Packs
```
## Description:

Instrument scores are appended to the instrument packs, regardless of whether
this results in a differently named duplicate file. 

This only deals with Adobe PDF (.pdf) files, and ignores all the other types
of music files, such as Lilypond, Sibelius, MuseScore, MusicXML, etc.. Each 
Song/Piece/Composition is in its own directory. The directory name may include
additional details in [] brackets such as type of arrangement, missing 
instruments, or composer.

## Instrument part naming conventions, in this order:

* The song name needs to be specified and unique and separated by ' - '
* The instrument name is embedded in the part file name and must be the same
  as the instrument name in the instrument pack. It follows the song's name.
* The seat number is optional, usually 1, 2, 3, etc.. If not specified, seat 1 is assumed.
* Key & clef optionally assist with the correct distribution, consider
  'Trombone 1 B-flat Treble Clef' and 'Trombone 1 C-Concert Bass Clef' 
  to fine-tune the musician's preferences and abilities.
* Further information is optional and will be preserved.

## File naming Example:
```
Song_name - Instrument seat_number key clef further_info etc.pdf
```
where: 

_Mandatory:_
* ```Song name```    - The bit until the last ' - ' (space-hyphen-space) character
                 combination
* ```Instrument```   - The name of the instrument.

_Optional:_
* ```seat number```  - 1, 2, 3, 4, or nothing
* ```key```          - The key that the sheet music has been transposed to relative
                 to C-Concert. Not to be confused with the composition key!
* ```clef etc.```    - The clef that the score is written in 
* ```further_info``` - Any other details, e.g. number of pages, version

## Implementation Example: 
```
Sway - Trombone 1 C-Concert Bass Clef (with trumpet cues).pdf
```

The instrument pack would need a directory called 
'Trombone 1 C-Concert Bass Clef', or 'Trombone 1 C-Concert', or 
'Trombone 1'. The latter case will receive all Trombone 1 scores, regardless
of key or clef.
'Trombone' will also work, and will receive the scores from all the seats
1, 2, 3, etc.. If a new instrument is introduced, make sure that a directory
for it exists in the 'Instrument Packs' directory.

Depending on the arrangement, there may be multiple seats for an instrument,
or just one, which will default to the first seat (a.k.a. instrument '1'), or
 none. Using this logic, it is also possible to combine numerous scored 
instruments into one, such as 'Synth', 'Piano' and 'Organ', which can all go
into an instrument pack for someone who plays all 3, called 
'Synth, Piano & Organ'. 

## Distribution Example:

The example below illustrates the various cases, where we have a music score
file for each instrument in the '${SMDIR}' directory:
```
+-Sheet Music
  |
  +-Four Seasons [Vivaldi][Quartet arrangement]
  | |
  | +-Four Seasons - Violin 1.pdf
  | +-Four Seasons - Violin 2.pdf
  | +-Four Seasons - Bratsche.pdf
  | +-Four Seasons - Cello.pdf
  |
  +-Trout Quintet [Vivaldi]
    |
    +-Trout Quintet - Violin 1.pdf
    +-Trout Quintet - Violin 2.pdf
    +-Trout Quintet - Bratsche 1.pdf
    +-Trout Quintet - Bratsche 2.pdf
    +-Trout Quintet - Cello.pdf
```
The instrument pack directories are prepared as follows in the 
'Instrument Packs' directory:
```
+-Sheet Music
|  |
:  +...
|
+-Instrument Packs
  |
  +-Violin 1
  |
  +-Violin 2
  |
  +-Violin 3
  |
  +-Bratsche 1
  |
  +-Bratsche 2
  |
  +-Cello
```
The instrument packs will be populated as follows:
```
+-Instrument Packs
  |
  +-Violin 1
  | |
  | +-Four Seasons - Violin 1.pdf
  | +-Trout Quintet - Violin 1.pdf
  |
  +-Violin 2
  | |
  | +-Four Seasons - Violin 2.pdf
  | +-Trout Quintet - Violin 2.pdf
  |
  +-Violin 3
  |
  +-Bratsche 1
  | |
  | +-Four Seasons - Bratsche.pdf
  | +-Trout Quintet - Bratsche 1.pdf
  |
  +-Bratsche 2
  | |
  | +-Trout Quintet - Bratsche 2.pdf
  |
  +-Cello
    |
    +-Four Seasons - Cello.pdf
    +-Trout Quintet - Cello.pdf
```
