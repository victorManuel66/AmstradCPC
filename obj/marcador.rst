ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 1.
Hexadecimal [16-Bits]



                              1 .area _CODE
                              2 
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 2.
Hexadecimal [16-Bits]



                              3 .include "cpctelera.h.s"
                              1 .globl cpct_disableFirmware_asm
                              2 .globl cpct_setVideoMode_asm
                              3 .globl cpct_setPalette_asm
                              4 .globl cpct_drawSprite_asm
                              5 .globl cpct_getScreenPtr_asm
                              6 .globl cpct_scanKeyboard_asm
                              7 .globl cpct_isKeyPressed_asm
                              8 .globl cpct_waitVSYNC_asm
                              9 .globl cpct_drawSolidBox_asm
                             10 .globl cpct_etm_setTileset2x4_asm
                             11 .globl cpct_etm_drawTileBox2x4_asm
                             12 .globl cpct_getRandom_xsp40_u8_asm
                             13 .globl cpct_akp_SFXInit_asm
                             14 .globl cpct_akp_musicInit_asm
                             15 .globl cpct_akp_musicPlay_asm
                             16 .globl cpct_akp_SFXPlay_asm
                             17 .globl cpct_akp_SFXStop_asm
                             18 .globl cpct_akp_stop_asm
                             19 .globl cpct_drawStringM0_asm
                             20 .globl cpct_setDrawCharM0_asm
                             21 .globl cpct_isAnyKeyPressed_asm
                             22 .globl cpct_drawCharM0_asm
                             23 .globl cpct_setInterruptHandler_asm
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 3.
Hexadecimal [16-Bits]



                              4 
   4B48 00                    5 decenas:  .db 0x00
   4B49 00                    6 centenas: .db 0x00
   4B4A 00                    7 millares: .db 0x00
   4B4B 00                    8 diezmil:  .db 0x00
                              9 
   4B4C                      10 suma::
   4B4C 21 48 4B      [10]   11     ld hl, #decenas
   4B4F 34            [11]   12     inc (hl)
   4B50 3E 0A         [ 7]   13     ld a, #0x0A
   4B52 BE            [ 7]   14     cp (hl)
   4B53 20 25         [12]   15     jr nz, guarda
   4B55 3E 00         [ 7]   16     ld  a, #0x00
   4B57 32 48 4B      [13]   17     ld (decenas), a
   4B5A 21 49 4B      [10]   18     ld hl, #centenas
   4B5D 34            [11]   19     inc (hl)
   4B5E 3E 0A         [ 7]   20     ld  a, #0x0A
   4B60 BE            [ 7]   21     cp (hl)
   4B61 20 17         [12]   22     jr nz, guarda
   4B63 3E 00         [ 7]   23     ld  a, #0x00
   4B65 32 49 4B      [13]   24     ld (centenas), a
   4B68 21 4A 4B      [10]   25     ld hl, #millares
   4B6B 34            [11]   26     inc (hl)
   4B6C 3E 0A         [ 7]   27     ld  a, #0x0A
   4B6E BE            [ 7]   28     cp (hl)
   4B6F 20 09         [12]   29     jr nz, guarda
   4B71 3E 00         [ 7]   30     ld a, #0x00
   4B73 32 4A 4B      [13]   31     ld (millares), a
   4B76 21 4B 4B      [10]   32     ld hl, #diezmil
   4B79 34            [11]   33     inc (hl)
   4B7A                      34 guarda:
   4B7A 3A 48 4B      [13]   35     ld  a, (decenas)
   4B7D CD 93 4B      [17]   36     call printdecena
   4B80 3A 49 4B      [13]   37     ld  a, (centenas)
   4B83 CD AB 4B      [17]   38     call printcentena
   4B86 3A 4A 4B      [13]   39     ld  a, (millares)
   4B89 CD C3 4B      [17]   40     call printmillares
   4B8C 3A 4B 4B      [13]   41     ld  a, (diezmil)
   4B8F CD DB 4B      [17]   42     call printdiezmil
                             43 
   4B92 C9            [10]   44     ret
                             45 
   4B93                      46 printdecena:
   4B93 C6 30         [ 7]   47     add #0x30                            ;; El codigo ASCII de '0'
   4B95 F5            [11]   48     push af
   4B96 21 02 00      [10]   49     ld hl, #0x0002
   4B99 CD D6 60      [17]   50     call cpct_setDrawCharM0_asm
   4B9C 11 00 C0      [10]   51     ld de, #0xC000
   4B9F 01 43 5C      [10]   52     ld bc, #0x5C43
   4BA2 CD F9 60      [17]   53     call cpct_getScreenPtr_asm
   4BA5 F1            [10]   54     pop af
   4BA6 5F            [ 4]   55     ld  e, a
   4BA7 CD 78 5F      [17]   56     call cpct_drawCharM0_asm
   4BAA C9            [10]   57     ret
                             58 
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 4.
Hexadecimal [16-Bits]



   4BAB                      59 printcentena:
   4BAB C6 30         [ 7]   60     add #0x30                            ;; El codigo ASCII de '0'
   4BAD F5            [11]   61     push af
   4BAE 21 02 00      [10]   62     ld hl, #0x0002
   4BB1 CD D6 60      [17]   63     call cpct_setDrawCharM0_asm
   4BB4 11 00 C0      [10]   64     ld de, #0xC000
   4BB7 01 3F 5C      [10]   65     ld bc, #0x5C3F
   4BBA CD F9 60      [17]   66     call cpct_getScreenPtr_asm
   4BBD F1            [10]   67     pop af
   4BBE 5F            [ 4]   68     ld  e, a
   4BBF CD 78 5F      [17]   69     call cpct_drawCharM0_asm
   4BC2 C9            [10]   70     ret
                             71 
   4BC3                      72 printmillares:
   4BC3 C6 30         [ 7]   73     add #0x30                            ;; El codigo ASCII de '0'
   4BC5 F5            [11]   74     push af
   4BC6 21 02 00      [10]   75     ld hl, #0x0002
   4BC9 CD D6 60      [17]   76     call cpct_setDrawCharM0_asm
   4BCC 11 00 C0      [10]   77     ld de, #0xC000
   4BCF 01 3B 5C      [10]   78     ld bc, #0x5C3B
   4BD2 CD F9 60      [17]   79     call cpct_getScreenPtr_asm
   4BD5 F1            [10]   80     pop af
   4BD6 5F            [ 4]   81     ld  e, a
   4BD7 CD 78 5F      [17]   82     call cpct_drawCharM0_asm
   4BDA C9            [10]   83     ret
                             84 
   4BDB                      85 printdiezmil:
   4BDB C6 30         [ 7]   86     add #0x30                            ;; El codigo ASCII de '0'
   4BDD F5            [11]   87     push af
   4BDE 21 02 00      [10]   88     ld hl, #0x0002
   4BE1 CD D6 60      [17]   89     call cpct_setDrawCharM0_asm
   4BE4 11 00 C0      [10]   90     ld de, #0xC000
   4BE7 01 37 5C      [10]   91     ld bc, #0x5C37
   4BEA CD F9 60      [17]   92     call cpct_getScreenPtr_asm
   4BED F1            [10]   93     pop af
   4BEE 5F            [ 4]   94     ld  e, a
   4BEF CD 78 5F      [17]   95     call cpct_drawCharM0_asm
   4BF2 C9            [10]   96     ret
