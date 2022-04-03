ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 1.
Hexadecimal [16-Bits]



                              1 .area _CODE
                              2 
   4BF3                       3 gameover::
   4BF3 47 41 4D 45 20 4F     4     .ascii "GAME OVER"
        56 45 52
   4BFC 00                    5     .db 0
   4BFD                       6 new::
   4BFD 6E 65 77              7     .ascii "new"
   4C00 00                    8     .db 0
   4C01                       9 escore::
   4C01 73 63 6F 72 65       10     .ascii "score"
   4C06 00                   11     .db 0
   4C07                      12 lives::
   4C07 6C 69 76 65 73       13     .ascii "lives"
   4C0C 00                   14     .db 0
   4C0D                      15 writeMenu1::
   4C0D 31 2E 2D 20 50 6C    16     .ascii "1.- Play"
        61 79
   4C15 00                   17     .db 0
   4C16                      18 writeMenu2::
   4C16 32 2E 2D 20 52 65    19     .ascii "2.- Redefine"
        64 65 66 69 6E 65
   4C22 00                   20     .db 0
   4C23                      21 writeMenu3::
   4C23 33 2E 2D 20 4A 6F    22     .ascii "3.- Joystick"
        79 73 74 69 63 6B
   4C2F 00                   23     .db 0
   4C30                      24 izqui::
   4C30 4C 65 66 74 20 20    25     .ascii "Left  :  "
        3A 20 20
   4C39 00                   26     .db 0
   4C3A                      27 dere::
   4C3A 52 69 67 68 74 20    28     .ascii "Right : "
        3A 20
   4C42 00                   29     .db 0
   4C43                      30 fue::
   4C43 46 69 72 65 20 20    31     .ascii "Fire  : "
        3A 20
   4C4B 00                   32     .db 0
   4C4C                      33 puntos::
   4C4C 30 30 30 30 30       34     .ascii "00000"
   4C51 00                   35     .db 0
   4C52                      36 nivel1::
   4C52 52 45 41 44 59 21    37     .ascii "READY!"
   4C58 00                   38     .db 0
                             39 
