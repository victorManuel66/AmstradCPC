ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 1.
Hexadecimal [16-Bits]



                              1 .module main
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



                              4 .include "Player.h.s"
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



                              5 .include "laser.h.s"
                              1 .globl draw_laser
                              2 .globl erase_laser
                              3 .globl update_laser
                              4 .globl posXlaserPtr
                              5 .globl posYlaserPtr
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 5.
Hexadecimal [16-Bits]



                              6 .include "enemy.h.s"
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
                             12 .globl finOleada
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 6.
Hexadecimal [16-Bits]



                              7 .include "laserColision.h.s"
                              1 .globl laser_colision
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 7.
Hexadecimal [16-Bits]



                              8 .include "enemy_colision.h.s"
                              1 .globl colision_enemy
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 8.
Hexadecimal [16-Bits]



                              9 .include "destruyePlayer.h.s"
                              1 .globl destruyePlayer
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 9.
Hexadecimal [16-Bits]



                             10 .include "mensajes.h.s"
                              1 .globl gameover
                              2 .globl escore
                              3 .globl new
                              4 .globl lives
                              5 .globl writeMenu1
                              6 .globl writeMenu2
                              7 .globl writeMenu3
                              8 .globl izqui
                              9 .globl dere
                             10 .globl fue
                             11 .globl puntos
                             12 .globl nivel1
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 10.
Hexadecimal [16-Bits]



                             11 .include "menu.h.s"
                              1 .globl menu
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 11.
Hexadecimal [16-Bits]



                             12 .include "destroyEnemy.h.s"
                              1 .globl destruyeEnemigo
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 12.
Hexadecimal [16-Bits]



                             13 .include "datos.h.s"
                     0014     1 tamagno_enemy =            0x14                               ;; Tama??o del array de entidades
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 13.
Hexadecimal [16-Bits]



                             14 .include "control.h.s"
                              1 .globl situaEnemigos
                              2 .globl reloj
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 14.
Hexadecimal [16-Bits]



                             15 
                             16 .area _DATA
                             17 .area _CODE
                             18 
                             19 .globl _tileset
                             20 .globl _fondo
                             21 .globl _Newexplo
                             22 .globl _spr
                             23 
                             24 
                             25 
                             26 
   4C59                      27 paleta:
   4C59 14 14 1D 18 0C 1C    28    .db 20,20,29,24,12,28,11,2,0,14,0,0,19,10,14,27
        0B 02 00 0E 00 00
        13 0A 0E 1B
                             29 
   4C69                      30 _main::
                             31    ;; Disable firmware to prevent it from interfering with string drawing
   4C69 CD F1 5F      [17]   32    call cpct_disableFirmware_asm
                             33    ;; Cambiamos a modo de video 0
   4C6C 0E 00         [ 7]   34    ld  c, #0x00
   4C6E CD B7 5F      [17]   35    call cpct_setVideoMode_asm
                             36    ;; Asignamos colores a la paleta
   4C71 21 59 4C      [10]   37    ld hl, #paleta
   4C74 11 10 00      [10]   38    ld de, #0x10
   4C77 CD 5C 55      [17]   39    call cpct_setPalette_asm
                             40    ;; Inicializar sonidos SFX
   4C7A 11 40 00      [10]   41    ld de, #_Newexplo
   4C7D CD 1D 5D      [17]   42    call cpct_akp_SFXInit_asm
   4C80 11 40 00      [10]   43    ld de, #_Newexplo
   4C83 CD 91 5C      [17]   44    call cpct_akp_musicInit_asm
                             45 
   4C86 CD 50 50      [17]   46    call menu
                             47 
   4C89                      48 newGame:
                             49    ;; Dibujar la pantalla
   4C89 21 66 48      [10]   50    ld hl, #_tileset
   4C8C CD 64 5F      [17]   51    call cpct_etm_setTileset2x4_asm
   4C8F 21 00 40      [10]   52    ld hl, #_fondo
   4C92 E5            [11]   53    push hl
   4C93 21 00 C0      [10]   54    ld hl, #0xC000
   4C96 E5            [11]   55    push hl
   4C97 01 00 00      [10]   56    ld bc, #0x0000
   4C9A 11 28 32      [10]   57    ld de, #0x3228
   4C9D 3E 28         [ 7]   58    ld  a, #0x28
   4C9F CD D8 5E      [17]   59    call cpct_etm_drawTileBox2x4_asm
                             60 
                             61    ;; Marcador
   4CA2 21 02 00      [10]   62    ld hl, #0x0002
   4CA5 CD C6 60      [17]   63    call cpct_setDrawCharM0_asm                                    ;; Colores del texto
                             64 
   4CA8 11 00 C0      [10]   65    ld de, #0xC000
   4CAB 01 3B 14      [10]   66    ld bc, #0x143B
   4CAE CD E9 60      [17]   67    call cpct_getScreenPtr_asm                                     ;; Posici??n de pantalla donde escribir
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 15.
Hexadecimal [16-Bits]



   4CB1 FD 21 FD 4B   [14]   68    ld iy, #new
   4CB5 CD AE 5D      [17]   69    call cpct_drawStringM0_asm
   4CB8 11 00 C0      [10]   70    ld de, #0xC000
   4CBB 01 37 1B      [10]   71    ld bc, #0x1B37
   4CBE CD E9 60      [17]   72    call cpct_getScreenPtr_asm                                     ;; Posici??n de pantalla donde escribir
   4CC1 FD 21 01 4C   [14]   73    ld iy, #escore
   4CC5 CD AE 5D      [17]   74    call cpct_drawStringM0_asm
   4CC8 11 00 C0      [10]   75    ld de, #0xC000
   4CCB 01 37 4E      [10]   76    ld bc, #0x4E37
   4CCE CD E9 60      [17]   77    call cpct_getScreenPtr_asm                                     ;; Posici??n de pantalla donde escribir
   4CD1 FD 21 01 4C   [14]   78    ld iy, #escore
   4CD5 CD AE 5D      [17]   79    call cpct_drawStringM0_asm
   4CD8 11 00 C0      [10]   80    ld de, #0xC000
   4CDB 01 37 8A      [10]   81    ld bc, #0x8A37
   4CDE CD E9 60      [17]   82    call cpct_getScreenPtr_asm                                     ;; Posici??n de pantalla donde escribir
   4CE1 FD 21 07 4C   [14]   83    ld iy, #lives                                                  
   4CE5 CD AE 5D      [17]   84    call cpct_drawStringM0_asm                                     ;; Escribe vidas
   4CE8 11 00 C0      [10]   85    ld de, #0xC000
   4CEB 01 37 5C      [10]   86    ld bc, #0x5C37
   4CEE CD E9 60      [17]   87    call cpct_getScreenPtr_asm
   4CF1 FD 21 4C 4C   [14]   88    ld iy, #puntos
   4CF5 CD AE 5D      [17]   89    call cpct_drawStringM0_asm                                    ;; Dibuja los puntos
                             90 
                             91    ;; dibujar las naves en el marcador
   4CF8 01 37 A6      [10]   92    ld bc, #0xA637                                                 ;; En B coordenada Y en C coordenada X
   4CFB 3E 03         [ 7]   93    ld  a, #0x03                                                   ;; El n??mero de naves a dibujar
   4CFD                      94 nextShip:
   4CFD F5            [11]   95    push af                                                        ;; Preservo A por que las llamadas a cpctelera lo corrompen
   4CFE C5            [11]   96    push bc                                                        ;; Lo mismo para BC
   4CFF 11 00 C0      [10]   97    ld de, #0xC000
   4D02 CD E9 60      [17]   98    call cpct_getScreenPtr_asm                                     ;; Posici??n de pantalla donde escribir
   4D05 EB            [ 4]   99    ex de, hl                                                      ;; Necesario por cpct_drawSprite
   4D06 21 5A 49      [10]  100    ld hl, #_spr                                                   ;; Direcci??n donde esta el sprite
   4D09 01 05 06      [10]  101    ld bc, #0x0605                                                 ;; B alto y C ancho del sprite
   4D0C CD 38 5E      [17]  102    call cpct_drawSprite_asm                                       ;; dibuja el sprite
   4D0F C1            [10]  103    pop bc
   4D10 3E 07         [ 7]  104    ld  a, #0x07
   4D12 81            [ 4]  105    add a, c
   4D13 4F            [ 4]  106    ld  c,a
   4D14 F1            [10]  107    pop af
   4D15 3D            [ 4]  108    dec  a
   4D16 20 E5         [12]  109    jr nz, nextShip
                            110 
   4D18                     111 principal:
   4D18 21 0E 00      [10]  112       ld hl, #0x000E
   4D1B CD C6 60      [17]  113       call cpct_setDrawCharM0_asm
   4D1E 11 00 C0      [10]  114       ld de, #0xC000
   4D21 01 10 4E      [10]  115       ld bc, #0x4E10
   4D24 CD E9 60      [17]  116       call cpct_getScreenPtr_asm                                     ;; Posici??n de pantalla donde escribir
   4D27 FD 21 52 4C   [14]  117       ld iy, #nivel1                                                 ;; Direcci??n donde se encuentra el mensaje
   4D2B CD AE 5D      [17]  118       call cpct_drawStringM0_asm
                            119  
   4D2E DD 21 21 4E   [14]  120    ld ix, #enemy
   4D32 CD 18 4B      [17]  121    call situaEnemigos
                            122 
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 16.
Hexadecimal [16-Bits]



   4D35                     123    main_loop:
   4D35 DD 21 88 49   [14]  124       ld ix, #Player                                     ;; IX contiene el inicio de los datos del Player    
   4D39 CD FE 49      [17]  125       call erase_player                                  ;; Borra el sprite de la posici??n actual
   4D3C DD 21 21 4E   [14]  126       ld ix, #enemy                                      ;; IX ahora apunta al inicio de los datos enemigo
   4D40 CD 1F 4F      [17]  127       call erase_enemy                                   ;; Borra al enemigo
                            128 
   4D43 DD 21 88 49   [14]  129       ld ix, #Player                                     ;; IX contiene el inicio de los datos del Player
   4D47 CD 96 49      [17]  130       call update_player                                 ;; Calcula la nueva posici??n
   4D4A DD 21 21 4E   [14]  131       ld ix, #enemy                                      ;; IX ahora apunta al inicio de los datos enemigo
   4D4E CD 54 4F      [17]  132       call update_enemy                                  ;; Calcula la nueva posici??n del enemigo
   4D51 DD 21 21 4E   [14]  133       ld ix, #enemy                                      ;; IX contiene el inicio de los datos del enemy
   4D55 CD 34 4A      [17]  134       call colision_enemy                                ;; Ver si los enemigos colisionan
                            135    
                            136 
   4D58 DD 21 88 49   [14]  137       ld ix, #Player                                     ;; IX contiene inicio de los datos del Player
   4D5C CD DD 49      [17]  138       call draw_player                                   ;; Dibuja el sprite en la nueva posici??n
   4D5F DD 21 21 4E   [14]  139       ld ix, #enemy                                      ;; IX ahora apunta al inicio de los datos del enemigo
   4D63 CD 9F 4E      [17]  140       call draw_enemy                                    ;; Dibuja al enemigo
                            141   
                            142 
   4D66 CD 18 4A      [17]  143       call disparando                                    ;; Comprobar si esta disparando
   4D69 7E            [ 7]  144       ld  a, (hl)
   4D6A FE 01         [ 7]  145       cp #0x01                                           ;; Si esta a uno es que tiene que disparar
   4D6C 20 10         [12]  146       jr nz, nodisparar
                            147 
   4D6E CD E2 4D      [17]  148       call erase_laser                                   ;; Borra el disparo
   4D71 CD FA 4D      [17]  149       call update_laser                                  ;; Actualiza el disparo
   4D74 CD C9 4D      [17]  150       call draw_laser                                    ;; Dibuja el disparo
   4D77 DD 21 21 4E   [14]  151       ld ix, #enemy
   4D7B CD D9 4F      [17]  152       call laser_colision                                ;; Ver si laser colision
                            153 
   4D7E                     154 nodisparar::
   4D7E CD C4 5F      [17]  155       call cpct_waitVSYNC_asm                            ;; Sincronizaci??n
   4D81 21 2D 4B      [10]  156       ld hl, #reloj
   4D84 CD 35 55      [17]  157       call cpct_setInterruptHandler_asm
   4D87 CD 87 55      [17]  158       call cpct_akp_musicPlay_asm                        ;; Tocar m??sica para usar los efectos de sonido
                            159 
                            160 
   4D8A DD 21 88 49   [14]  161       ld ix, #Player  
   4D8E DD 7E 06      [19]  162       ld  a, 6(ix)                                       ;; El n??mero de vidas restantes
   4D91 FE 00         [ 7]  163       cp  #0x00                                          ;; Ver si estan a cero las vidas
   4D93 20 A0         [12]  164       jr nz, main_loop                                   ;; Continua mientras haya vidas
                            165 
                            166 
   4D95 21 04 00      [10]  167       ld hl, #0x0004
   4D98 CD C6 60      [17]  168       call cpct_setDrawCharM0_asm
                            169 
   4D9B 11 00 C0      [10]  170       ld de, #0xC000                                     ;; Inicio de la memoria de video
   4D9E 01 0A 64      [10]  171       ld bc, #0x640A                                     ;; Coordenadas X y Y donde escribir
   4DA1 CD E9 60      [17]  172       call cpct_getScreenPtr_asm                         ;; Obtener direcci??n de momoria de video
                            173       
   4DA4 FD 21 F3 4B   [14]  174       ld iy, #gameover
   4DA8 CD AE 5D      [17]  175       call cpct_drawStringM0_asm                         ;; Excribe el mensaje
                            176 
   4DAB CD EA 5C      [17]  177       call cpct_akp_stop_asm                             ;; Para el sonido
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 17.
Hexadecimal [16-Bits]



                            178 
   4DAE                     179 espera:
   4DAE CD FB 60      [17]  180       call cpct_scanKeyboard_asm                         ;; Escanear al teclado
   4DB1 CD AA 5F      [17]  181       call cpct_isAnyKeyPressed_asm                      ;; Ver si se pulso una tecla
   4DB4 FE 00         [ 7]  182       cp #0x00                                           ;; Si en A hay un cero es que no se pulso una tecla
   4DB6 28 F6         [12]  183       jr  z, espera                                      ;; vuelve a espera hasta que se pulse una tecla
                            184 
   4DB8 CD 50 50      [17]  185       call menu
                            186 
   4DBB DD 21 88 49   [14]  187       ld ix, #Player                                     ;; Puntero al inicio de datos del jugador
   4DBF DD 36 06 03   [19]  188       ld 6(ix), #0x03                                    ;; N??mero de vidas de nuevo a tre
                            189 
                            190 
   4DC3 C3 89 4C      [10]  191       jp newGame                                         ;; Jugar de nuevo
                            192 
