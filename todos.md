I have to have a place where I can add TODOs, that are not automatically visible ....

* Internet (only possible when on the internet)
    * How to add a version number in Grunt-Files to some tasks
        * Version for the ZIP-File
        * Where do we need a version as well? A search brings the following results:
            * docu/*.html files: How to include version in HTML (==> Template parser)
        * `package.json` already contains a definition of `version`, so it should at least possible
        to add the version inside the Grunt build process
    * How to add that version in the documentation (possibly in the examples)?
    * How to exclude `node_modules` (normally) from the search process and result in IntelliJ
    * chess.js should know when to castle, but I did not find an API for that. What do I have to do? Look that up in the web site.
      * Found in code `move.flags & BITS.KSIDE_CASTLE`
      seems to be the same as "castling king side allowed"
      * Ditto for move.flags & BITS.QSIDE_CASTLE. Do I
      have access to the move flags? And how to decide (without any move) if castling is allowed or not? Does not work ...

## Restructure Code

The code at the moment is not easy to understand, and parts of it lays in the mis-structure that happened over time.
Try to restructure the code to get the following:

* functions are only inside that scope available where they should be used.
* functions that are only used once could be "inlined" in some way.
* Extract common parameters that go everywhere (constant!) or are part of the configuration (could be computed all time), and see what to do with it.

 * Use Font Awesome in a local copy, not the net version. This is used for all other resources, so why not here ...

### Avoid hide HTML

Instead, implement for each function exactly what is neeeded:

* board: Board only
* view: Viewer including pgn display, buttons for moving
* print: Board only as part of pgn display, no buttons
* edit: as view, additional edit and comment buttons

This means that the whole `generateHTML` has to be broken in different parts, that may be added or not (on demand).
And that the whole structure is more malleable, so anyone could construct their own structure. What are the parts
that have to be kepts so that everything works again?

### Add features to board

#### notation

notation should not only be true or false, but also possible outside of the board. Additionally, the position
could be signaled as well. The following is normal:

* combinations of N,S,E,W,C like N, NW, NC
* Meaning
  * N == NC
  * NC == top of the board, there the center
  * NW == top of the board, there the left
* This should be independent of the size of the board (and the fields) working.
* This would also allow to add a color to the board, which will shine through on the border (if you want).
  
## Documentation

* Add documentation how to create a base example: example HTML file for the 4 different cases (board, view, edit, print),
  link that file from the documentation, explain the base parts in it.

## Examples

* Try to find as minimal as possible examples so that it is easier to demonstrate the different features.
* Try to rename the example files, document (in HTML) what the purpose of each example is.
* Try to find one base example for each 'mode' like pgnBoard, pgnView, pgnEdit and pgnPrint. This will help debugging
  nasty problems, and find the reasons much easier.
* Try to find nice example games, and link them to the original source.
* Include in the example files the nice examples given somewhere, not the contrived ones I already have ...

## Tickets

#### Missing discriminator

* Check if there is a functionality to get the discriminator upfront (by using the chess.js library)
* See the function get_disambiguator in the library. So should work, find a test to check that (as unit test).
* Find at least the following examples for that:
  * Normal moves where no diammbiguator is needed.
  * Normal situation where the disambiguator has to be used.
  * Difficult situation where 2 moves could be possible, but one is not allowed (because the king is in check then).
