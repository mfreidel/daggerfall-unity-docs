# reveal

## Adding dungeons to the province

    reveal aPlace

This action reveals a remote dungeon associated with a quest on the fast travel map of the current province.


## Adding dungeons to the world map

    reveal aPlace in province aProvince# at magicNumber

This action adds a previously invisible location to the world map. aProvince# is the number for the province map on which to reveal aPlace. magicNumber is the key to the entry for aPlace in the **Map Table** of the specific province.

Magic numbers for hidden dungeon sites can be determined from the entries in arena2/maps.bsa. You can use the \`atlas' tool to create a prototype of a reveal command for any location on the fast travel map. For completeness, the method is explained below, but using the \`atlas' tool is usually easier and less error-prone.

Each province has four (4) members in maps.bsa: _mapnames.###_, _maptable.###_, _mappitem.###_, and _mapditem.###_, where _###_ is the number of a specific **Daggerfall** province.

The mapnames entry for tbe desired dungeon can be used to determine the maptable entry with the proper magic number for it in the following way:

1.  Find the offset in _mapnames_ where the desired dungeon name begins
2.  Subtract 4 from the offset and divide by 32. The remainder should be 0.
3.  Multiply the quotient by 17.
4.  This is the proper offset in maptable from which to obtain the magic number. (Offsets are numbered from 0)
5.  The five hexadecimal digits starting from offset are used to make the magic number.
6.  Write down the 5 hexadecimal digits from left to right.
7.  Reverse the byte order of these three \`virtual' bytes.
8.  This is the magic number to use with the reveal action. Preface this number with 0x to make the scenario compiler accept base 16 integers.

Here is the method applied to _Castle Necromoghan_ in _Daggerfall_ province. _Daggerfall_'s province number is 17, so we're interested in the maptable.017 and mapnames.017 members of maps.bsa.

Searching mapnames.017 for _Castle Necromoghan_ shows 0x124 for the first offset.
((0x124 - 4) / 32) \* 17 = 0x99.
Looking at offset 0x99 in maptable.017 shows 8f 08 2.
Reversing the bytes we get 0x2088f or 133263 decimal. So the reveal command would be

    reveal aPlace in province 17 at 133263
