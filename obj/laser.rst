ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 1.
Hexadecimal [16-Bits]



                              1 .area _CODE
                              2 
                              3 .globl _laser
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 2.
Hexadecimal [16-Bits]



                              4 .include "cpctelera.h.s"
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



                              5 .include "Player.h.s"
                              1 .globl draw_player
                              2 .globl update_player
                              3 .globl erase_player
                              4 .globl disparando
                              5 .globl posXplayerPtr
                              6 .globl posYenemyPtr
                              7 .globl Player
                              8 .globl TeclaIz
                              9 .globl TeclaDe
                             10 .globl TeclaDi
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 4.
Hexadecimal [16-Bits]



                              6 .include "main.h.s"
                              1 .globl nodisparar
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 5.
Hexadecimal [16-Bits]



                              7 
   4DC6 FC                    8 velocidad: .db -4                                         ;; Velocidad del disparo (0xFC)
                              9 
   4DC7 26                   10 laserX:    .db 0x26                                       ;; Posición X del disparo
   4DC8 BD                   11 laserY:    .db 189                                        ;; Posición Y del disparo (0xBD)
                             12 
   4DC9                      13 draw_laser::
   4DC9 11 00 C0      [10]   14     ld de, #0xC000
   4DCC 3A C7 4D      [13]   15     ld  a, (laserX)
   4DCF 4F            [ 4]   16     ld  c, a
   4DD0 3A C8 4D      [13]   17     ld  a,(laserY)
   4DD3 47            [ 4]   18     ld  b, a
   4DD4 CD E9 60      [17]   19     call cpct_getScreenPtr_asm
                             20 
   4DD7 EB            [ 4]   21     ex de,hl
   4DD8 21 46 49      [10]   22     ld hl, #_laser
   4DDB 01 01 04      [10]   23     ld bc, #0x0401
   4DDE CD 38 5E      [17]   24     call cpct_drawSprite_asm
                             25 
   4DE1 C9            [10]   26     ret
                             27 
   4DE2                      28 erase_laser::
   4DE2 11 00 C0      [10]   29     ld de, #0xC000
   4DE5 3A C7 4D      [13]   30     ld  a, (laserX)
   4DE8 4F            [ 4]   31     ld  c, a
   4DE9 3A C8 4D      [13]   32     ld  a,(laserY)
   4DEC 47            [ 4]   33     ld  b, a
   4DED CD E9 60      [17]   34     call cpct_getScreenPtr_asm
                             35 
   4DF0 EB            [ 4]   36     ex de, hl
   4DF1 3E 00         [ 7]   37     ld a, #0x00
   4DF3 01 01 04      [10]   38     ld bc, #0x0401
   4DF6 CD 01 60      [17]   39     call cpct_drawSolidBox_asm
                             40 
   4DF9 C9            [10]   41     ret
                             42 
   4DFA                      43 update_laser::
   4DFA 21 C6 4D      [10]   44     ld hl, #velocidad
   4DFD 3A C8 4D      [13]   45     ld  a, (laserY)
   4E00 86            [ 7]   46     add a,(hl)
   4E01 FE 09         [ 7]   47     cp #9                                         ;; Ver si ha llegado al final de la pantalla
   4E03 28 07         [12]   48     jr z, destruirLaser
   4E05 32 C8 4D      [13]   49     ld (laserY), a
   4E08 CD E2 4D      [17]   50     call erase_laser                              ;; Borrar para que no quede ratro
   4E0B C9            [10]   51     ret
   4E0C                      52 destruirLaser:
   4E0C 3E BD         [ 7]   53     ld a, #0xBD
   4E0E 32 C8 4D      [13]   54     ld (laserY), a                                ;; Coordenada Y del laser reseteada a #0xBD
   4E11 CD 18 4A      [17]   55     call disparando
   4E14 AF            [ 4]   56     xor  a
   4E15 77            [ 7]   57     ld (hl), a                                    ;; La posición de memoria disparando reseteada a cero
   4E16 C3 7E 4D      [10]   58     jp nodisparar                                 ;; Borra el último dibujo del sprite, y me da la impresion de que esto no esta muy bien
                             59 
                             60 
   4E19                      61 posXlaserPtr::
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 6.
Hexadecimal [16-Bits]



   4E19 21 C7 4D      [10]   62     ld hl, #laserX                                 ;; Devuelvo la dirección de laserX en HL
   4E1C C9            [10]   63     ret
                             64 
   4E1D                      65 posYlaserPtr::
   4E1D 21 C8 4D      [10]   66     ld hl, #laserY                                 ;; Devuelve la dirección de laserY en HL
   4E20 C9            [10]   67     ret
