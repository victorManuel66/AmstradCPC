ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 1.
Hexadecimal [16-Bits]



                              1 .module enemy_colision
                              2 .area _CODE
                              3 
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 2.
Hexadecimal [16-Bits]



                              4 .include "datos.h.s"
                     0014     1 tamagno_enemy =            0x14                               ;; Tamaño del array de entidades
                              2 
                              3 
                              4 
                              5 ;; Constantes del array del enemigo
                     0000     6 .equ enemiX,      0
                     0001     7 .equ enemiY,      1
                     0002     8 .equ enemiAlto,   2
                     0003     9 .equ enemiAncho,  3
                     0004    10 .equ SpriteBajo,  4
                     0005    11 .equ SpriteAlto,  5
                     0006    12 .equ enemiVelo,   6
                     0007    13 .equ temporiza,   7
                     0008    14 .equ StatusAni,   8
                     0009    15 .equ frame1Bajo,  9
                     000A    16 .equ frame1Alto, 10
                     000B    17 .equ frame2Bajo, 11
                     000C    18 .equ frame2Alto, 12
                     000D    19 .equ frame3Bajo, 13
                     000E    20 .equ frame3Alto, 14
                     000F    21 .equ frame4Bajo, 15
                     0010    22 .equ frame4Alto, 16
                     0011    23 .equ frame5Bajo, 17
                     0012    24 .equ frame5Alto, 18
                     0013    25 .equ conVida,    19 
                             26 
                             27 ;; Constantes del Player
                     0000    28 .equ PlayX,      0
                     0001    29 .equ PlayY,      1
                     0002    30 .equ PlayAlto,   2
                     0003    31 .equ PlayAncho,  3
                     0004    32 .equ SprPlayLo,  4
                     0005    33 .equ SprPlayHi,  5
                     0006    34 .equ PlayVidas,  6
                     0007    35 .equ FPS,        7
                             36 
                             37 
                             38 
                             39 
                             40 
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



                              6 .include "cpctelera.h.s"
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 5.
Hexadecimal [16-Bits]



                              7 .include "destroyEnemy.h.s"
                              1 .globl destruyeEnemigo
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 6.
Hexadecimal [16-Bits]



                              8 .include "destruyePlayer.h.s"
                              1 .globl destruyePlayer
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 7.
Hexadecimal [16-Bits]



                              9 .include "enemy.h.s"
                              1 .globl draw_enemy
                              2 .globl calXenemy
                              3 .globl erase_enemy
                              4 .globl update_enemy
                              5 .globl posXenemyPtr
                              6 .globl posYenemyPtr
                              7 .globl enemy
                              8 .globl enemy2
                              9 .globl enemy3
                             10 .globl num_enemigos
                             11 .globl oleada
                             12 ;.globl lastNum_enemies
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 8.
Hexadecimal [16-Bits]



                             10 
                             11 ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
                             12 ;; ESTA RUTINA SÓLO COMPRUEBA SI EL ENEMIGO CHOCA CONTRA EL PLAYER ;;
                             13 ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
                             14 
   4A34                      15 colision_enemy::
   4A34 FD 21 85 4E   [14]   16     ld iy, #num_enemigos
   4A38 FD 5E 00      [19]   17     ld  e, 0(iy)                         
   4A3B                      18 sigui_enemy:
   4A3B D5            [11]   19     push de                                        ;; Preservo E 
   4A3C CD 4A 4A      [17]   20     call colision                                  ;; Borrar una entidad enemigo enemigo
   4A3F D1            [10]   21     pop  de                                        ;; Recupero número de entidades enemigas
   4A40 1D            [ 4]   22     dec e                                          ;; Resta uno al total de entidades enemigas
   4A41 C8            [11]   23     ret z                                          ;; Si no quedan enemigos vuelve
   4A42 01 14 00      [10]   24     ld bc, #tamagno_enemy                          ;; El tamaño de los datos de un enemigo
   4A45 DD 09         [15]   25     add ix,bc                                      ;; Se suma a Ix oara deslazar el puntero
   4A47 18 F2         [12]   26     jr sigui_enemy                                 ;; Siguiente enemigo a borrar
   4A49 C9            [10]   27     ret
                             28 
   4A4A                      29 colision:
   4A4A DD 7E 08      [19]   30     ld  a, StatusAni(ix)                           ;; Comprobar el estado de la animación 
   4A4D FE 00         [ 7]   31     cp #0x00                                       ;; Si no es cero es que esta explotando 
   4A4F C0            [11]   32     ret nz                                         ;; Por lo tanto vuelve por que no hay que comprobar colosiones
   4A50 DD 46 00      [19]   33     ld  b, enemiX(ix)                              ;; Posición X del enemigo
   4A53 DD 7E 03      [19]   34     ld  a, enemiAncho(ix)                          ;; Ancho del enemigo
   4A56 80            [ 4]   35     add a, b                                       ;; Se suman
   4A57 47            [ 4]   36     ld  b, a                                       ;; En resultado se guarda en B
   4A58 CD 14 4A      [17]   37     call posXplayerPtr                             ;; Obtenemos la posición X del player
   4A5B 7E            [ 7]   38     ld  a, (hl)                                    ;; Guardar la coordena X en A
   4A5C B8            [ 4]   39     cp  b                                          ;; Las comparamos
   4A5D 28 03         [12]   40     jr  z, verSiMenor                              ;; Si es igual 
   4A5F 38 01         [12]   41     jr  c, verSiMenor                              ;; o mayor
   4A61 C9            [10]   42     ret                                            ;; Si no lo son, no puede haber colisión
   4A62                      43 verSiMenor:
   4A62 DD 46 00      [19]   44     ld  b, 0(ix)                                   ;; Posición X del enemigo
   4A65 C6 05         [ 7]   45     add a, #0x05                                   ;; Ancho del player (mejorable)
   4A67 B8            [ 4]   46     cp  b                                          ;; Comparar
   4A68 28 03         [12]   47     jr  z, verSiColY                               ;; Si es igual
   4A6A 30 01         [12]   48     jr  nc, verSiColY                              ;; o menor
   4A6C C9            [10]   49     ret                                            ;; Si llega aquí no puede haber colisión
                             50 
   4A6D                      51 verSiColY:
   4A6D DD 46 01      [19]   52     ld  b, enemiY(ix)                              ;; La coordenada Y del sprite del enemigo en B
   4A70 DD 7E 02      [19]   53     ld  a, enemiAlto(ix)                           ;; La altura del sprite del enemigo en A
   4A73 80            [ 4]   54     add a, b                                       ;; Se suma a la coordenada X del enemigo, resultado en A
   4A74 47            [ 4]   55     ld  b, a                                       ;; Lo guardo en B
   4A75 3E C1         [ 7]   56     ld  a, #0xC1                                   ;; C1 es la posición Y del player, que nunca cambia durante el juego (mejor usar constantes)
   4A77 B8            [ 4]   57     cp  b                                          ;; Se comparan, y si son iguales
   4A78 28 03         [12]   58     jr  z, choque                                  ;; Colisionaron
   4A7A 38 01         [12]   59     jr  c, choque                                  ;; Si A es menor que B tambien hay colisión
   4A7C C9            [10]   60     ret
   4A7D                      61 choque:                                            ;; Si llego hasta aquí es que hubo colisión
   4A7D CD 2B 4A      [17]   62     call destruyeEnemigo
   4A80 CD 87 4A      [17]   63     call destruyePlayer
                             64 
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 9.
Hexadecimal [16-Bits]



                             65  
                             66   
   4A83 C9            [10]   67     ret
