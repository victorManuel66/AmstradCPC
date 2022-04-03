ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 1.
Hexadecimal [16-Bits]



                              1 .module enemy
                              2 .area _CODE
                              3 
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



                              5 .include "datos.h.s"
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 4.
Hexadecimal [16-Bits]



                              6 
                              7 .globl _enemigo
                              8 .globl _explo1
                              9 .globl _explo2
                             10 .globl _explo3
                             11 .globl _explo4
                             12 .globl _explo5
                             13 
                             14 .macro EntEnemigas name, x, y, h, w, _enemigo, velo, tiem, sta, animac1, animac2, animac3, animac4, animac5, alive
                             15   name::
                             16     name'X:      .db x                      ;; Coordenada X del enemigo
                             17     name'Y:      .db y                      ;; Coordenada Y del enemigo
                             18     name'H:      .db h                      ;; Altura del sprite del enemigo
                             19     name'W:      .db w                      ;; Ancho del sprite del enemigo
                             20     name'spr:    .dw _enemigo               ;; Dirección del sprite del enemigo
                             21     name'vel:    .db velo                   ;; Velocidad del enemigo
                             22     name'tiemp:  .db tiem                   ;; Tiempo entre fotográmas
                             23     name'status: .db sta                    ;; Estado de la animación al explotar
                             24     name'ani1:   .dw animac1                ;; Direccion sprite animación 1
                             25     name'ani2:   .dw animac2                ;; Direccion sprite animación 2 
                             26     name'ani3:   .dw animac3                ;; Direccion sprite animación 3
                             27     name'ani4:   .dw animac4                ;; Direccion sprite animación 4
                             28     name'ani5:   .dw animac5                ;; Direccion sprite animación 5
                             29     name'vivo:   .db alive                  ;; 00 indica que esta muerto, 01 indica que esta viva
                             30 .endm
                             31 
   4E26                      32 EntEnemigas enemy,  0x00, 0x09, 0x08, 0x03, _enemigo, 0x01, 0x00, 0x00, _explo1, _explo2, _explo3, _explo4, _explo5, 0x01
   0000                       1   enemy::
   4E26 00                    2     enemyX:      .db 0x00                      ;; Coordenada X del enemigo
   4E27 09                    3     enemyY:      .db 0x09                      ;; Coordenada Y del enemigo
   4E28 08                    4     enemyH:      .db 0x08                      ;; Altura del sprite del enemigo
   4E29 03                    5     enemyW:      .db 0x03                      ;; Ancho del sprite del enemigo
   4E2A 2E 49                 6     enemyspr:    .dw _enemigo               ;; Dirección del sprite del enemigo
   4E2C 01                    7     enemyvel:    .db 0x01                   ;; Velocidad del enemigo
   4E2D 00                    8     enemytiemp:  .db 0x00                   ;; Tiempo entre fotográmas
   4E2E 00                    9     enemystatus: .db 0x00                    ;; Estado de la animación al explotar
   4E2F 16 49                10     enemyani1:   .dw _explo1                ;; Direccion sprite animación 1
   4E31 FE 48                11     enemyani2:   .dw _explo2                ;; Direccion sprite animación 2 
   4E33 E6 48                12     enemyani3:   .dw _explo3                ;; Direccion sprite animación 3
   4E35 CE 48                13     enemyani4:   .dw _explo4                ;; Direccion sprite animación 4
   4E37 B6 48                14     enemyani5:   .dw _explo5                ;; Direccion sprite animación 5
   4E39 01                   15     enemyvivo:   .db 0x01                  ;; 00 indica que esta muerto, 01 indica que esta viva
   4E3A                      33 EntEnemigas enemy2, 0x00, 0x09, 0x08, 0x03, _enemigo, 0x01, 0x00, 0x00, _explo1, _explo2, _explo3, _explo4, _explo5, 0x01
   0014                       1   enemy2::
   4E3A 00                    2     enemy2X:      .db 0x00                      ;; Coordenada X del enemigo
   4E3B 09                    3     enemy2Y:      .db 0x09                      ;; Coordenada Y del enemigo
   4E3C 08                    4     enemy2H:      .db 0x08                      ;; Altura del sprite del enemigo
   4E3D 03                    5     enemy2W:      .db 0x03                      ;; Ancho del sprite del enemigo
   4E3E 2E 49                 6     enemy2spr:    .dw _enemigo               ;; Dirección del sprite del enemigo
   4E40 01                    7     enemy2vel:    .db 0x01                   ;; Velocidad del enemigo
   4E41 00                    8     enemy2tiemp:  .db 0x00                   ;; Tiempo entre fotográmas
   4E42 00                    9     enemy2status: .db 0x00                    ;; Estado de la animación al explotar
   4E43 16 49                10     enemy2ani1:   .dw _explo1                ;; Direccion sprite animación 1
   4E45 FE 48                11     enemy2ani2:   .dw _explo2                ;; Direccion sprite animación 2 
   4E47 E6 48                12     enemy2ani3:   .dw _explo3                ;; Direccion sprite animación 3
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 5.
Hexadecimal [16-Bits]



   4E49 CE 48                13     enemy2ani4:   .dw _explo4                ;; Direccion sprite animación 4
   4E4B B6 48                14     enemy2ani5:   .dw _explo5                ;; Direccion sprite animación 5
   4E4D 01                   15     enemy2vivo:   .db 0x01                  ;; 00 indica que esta muerto, 01 indica que esta viva
   4E4E                      34 EntEnemigas enemy3, 0x00, 0x09, 0x08, 0x03, _enemigo, 0x01, 0x00, 0x00, _explo1, _explo2, _explo3, _explo4, _explo5, 0x01
   0028                       1   enemy3::
   4E4E 00                    2     enemy3X:      .db 0x00                      ;; Coordenada X del enemigo
   4E4F 09                    3     enemy3Y:      .db 0x09                      ;; Coordenada Y del enemigo
   4E50 08                    4     enemy3H:      .db 0x08                      ;; Altura del sprite del enemigo
   4E51 03                    5     enemy3W:      .db 0x03                      ;; Ancho del sprite del enemigo
   4E52 2E 49                 6     enemy3spr:    .dw _enemigo               ;; Dirección del sprite del enemigo
   4E54 01                    7     enemy3vel:    .db 0x01                   ;; Velocidad del enemigo
   4E55 00                    8     enemy3tiemp:  .db 0x00                   ;; Tiempo entre fotográmas
   4E56 00                    9     enemy3status: .db 0x00                    ;; Estado de la animación al explotar
   4E57 16 49                10     enemy3ani1:   .dw _explo1                ;; Direccion sprite animación 1
   4E59 FE 48                11     enemy3ani2:   .dw _explo2                ;; Direccion sprite animación 2 
   4E5B E6 48                12     enemy3ani3:   .dw _explo3                ;; Direccion sprite animación 3
   4E5D CE 48                13     enemy3ani4:   .dw _explo4                ;; Direccion sprite animación 4
   4E5F B6 48                14     enemy3ani5:   .dw _explo5                ;; Direccion sprite animación 5
   4E61 01                   15     enemy3vivo:   .db 0x01                  ;; 00 indica que esta muerto, 01 indica que esta viva
   4E62                      35 EntEnemigas enemy4, 0x00, 0x09, 0x08, 0x03, _enemigo, 0x01, 0x00, 0x00, _explo1, _explo2, _explo3, _explo4, _explo5, 0x01
   003C                       1   enemy4::
   4E62 00                    2     enemy4X:      .db 0x00                      ;; Coordenada X del enemigo
   4E63 09                    3     enemy4Y:      .db 0x09                      ;; Coordenada Y del enemigo
   4E64 08                    4     enemy4H:      .db 0x08                      ;; Altura del sprite del enemigo
   4E65 03                    5     enemy4W:      .db 0x03                      ;; Ancho del sprite del enemigo
   4E66 2E 49                 6     enemy4spr:    .dw _enemigo               ;; Dirección del sprite del enemigo
   4E68 01                    7     enemy4vel:    .db 0x01                   ;; Velocidad del enemigo
   4E69 00                    8     enemy4tiemp:  .db 0x00                   ;; Tiempo entre fotográmas
   4E6A 00                    9     enemy4status: .db 0x00                    ;; Estado de la animación al explotar
   4E6B 16 49                10     enemy4ani1:   .dw _explo1                ;; Direccion sprite animación 1
   4E6D FE 48                11     enemy4ani2:   .dw _explo2                ;; Direccion sprite animación 2 
   4E6F E6 48                12     enemy4ani3:   .dw _explo3                ;; Direccion sprite animación 3
   4E71 CE 48                13     enemy4ani4:   .dw _explo4                ;; Direccion sprite animación 4
   4E73 B6 48                14     enemy4ani5:   .dw _explo5                ;; Direccion sprite animación 5
   4E75 01                   15     enemy4vivo:   .db 0x01                  ;; 00 indica que esta muerto, 01 indica que esta viva
   4E76                      36 EntEnemigas enemy5, 0x00, 0x09, 0x08, 0x03, _enemigo, 0x01, 0x00, 0x00, _explo1, _explo2, _explo3, _explo4, _explo5, 0x01
   0050                       1   enemy5::
   4E76 00                    2     enemy5X:      .db 0x00                      ;; Coordenada X del enemigo
   4E77 09                    3     enemy5Y:      .db 0x09                      ;; Coordenada Y del enemigo
   4E78 08                    4     enemy5H:      .db 0x08                      ;; Altura del sprite del enemigo
   4E79 03                    5     enemy5W:      .db 0x03                      ;; Ancho del sprite del enemigo
   4E7A 2E 49                 6     enemy5spr:    .dw _enemigo               ;; Dirección del sprite del enemigo
   4E7C 01                    7     enemy5vel:    .db 0x01                   ;; Velocidad del enemigo
   4E7D 00                    8     enemy5tiemp:  .db 0x00                   ;; Tiempo entre fotográmas
   4E7E 00                    9     enemy5status: .db 0x00                    ;; Estado de la animación al explotar
   4E7F 16 49                10     enemy5ani1:   .dw _explo1                ;; Direccion sprite animación 1
   4E81 FE 48                11     enemy5ani2:   .dw _explo2                ;; Direccion sprite animación 2 
   4E83 E6 48                12     enemy5ani3:   .dw _explo3                ;; Direccion sprite animación 3
   4E85 CE 48                13     enemy5ani4:   .dw _explo4                ;; Direccion sprite animación 4
   4E87 B6 48                14     enemy5ani5:   .dw _explo5                ;; Direccion sprite animación 5
   4E89 01                   15     enemy5vivo:   .db 0x01                  ;; 00 indica que esta muerto, 01 indica que esta viva
                             37 
   4E8A 05                   38 num_enemigos::             .db 0x05                           ;; Número de enemigos en pantalla a la vez
   4E8B 14                   39 oleada::                   .db 0x14                           ;; Número total de la primera oleada de enemigos
   4E8C 00                   40 finOleada::                .db 0x00                           ;; Se mataron todos los enemigos de la oleada
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 6.
Hexadecimal [16-Bits]



                             41  
                             42 
                             43 ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
                             44 ;;          ESTA RUTINA NECESITA EN IX LA DIRECCIÓN DE INICIO DE LOS DATOS DE LA ENTIDAD ENEMIGO            ;;
                             45 ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;                   
   4E8D                      46 calXenemy::
   4E8D CD DC 5F      [17]   47     call cpct_getRandom_xsp40_u8_asm                   ;; Devuelve en A un numero pseudo aleatorio de 8 bits
   4E90 FE 05         [ 7]   48     cp #0x05                                           ;; Comparo con el valor mímimo
   4E92 38 06         [12]   49     jr c, menorCuatro                                  ;; Si es menor que cuatro salta a menorCuatro
   4E94 FE 2F         [ 7]   50     cp #0x2f                                           ;; Comparo con 48 decimal
   4E96 30 06         [12]   51     jr nc, mayor47                                     ;; Si no hay acarreo es que es mayor de 48
   4E98 18 06         [12]   52     jr fin                                             ;; Si llego aquí es porque es mayor que 4 y menor de 48                         
   4E9A                      53 menorCuatro:
   4E9A C6 05         [ 7]   54     add a,#0x05
   4E9C 18 02         [12]   55     jr fin
   4E9E                      56 mayor47:
   4E9E 18 ED         [12]   57     jr calXenemy
   4EA0                      58 fin:
   4EA0 DD 77 00      [19]   59     ld enemiX(ix), a                                   ;; Guarda la nueva coordenada X para el enemigo
   4EA3 C9            [10]   60     ret
                             61 
   4EA4                      62 draw_enemy::
   4EA4 FD 21 8A 4E   [14]   63     ld  iy, #num_enemigos
   4EA8 FD 5E 00      [19]   64     ld  e, 0(iy)
   4EAB                      65 sigui_enemy_draw:                         
   4EAB D5            [11]   66     push de                                            ;; draw_enemy_sprite corrompe DE
   4EAC CD BA 4E      [17]   67     call draw_enemy_sprite                             ;; Dibujar una entidad enemigo
   4EAF D1            [10]   68     pop de                                             ;; Recuperar DE con el total de entidades enemigas restantes
   4EB0 1D            [ 4]   69     dec e                                              ;; Resta uno al total de entidades enemigas
   4EB1 C8            [11]   70     ret z                                              ;; Si no quedan enemigos vuelve
   4EB2 01 14 00      [10]   71     ld bc, #tamagno_enemy                              ;; El tamaño de los datos de un enemigo
   4EB5 DD 09         [15]   72     add ix,bc                                          ;; Se suma a Ix oara deslazar el puntero
   4EB7 18 F2         [12]   73     jr sigui_enemy_draw                                ;; Siguiente enemigo a dibujar
   4EB9 C9            [10]   74     ret
                             75 
   4EBA                      76 draw_enemy_sprite:
   4EBA DD 7E 13      [19]   77     ld  a, conVida(ix)                                 ;; Ver si el enemigo esta vivo
   4EBD FE 00         [ 7]   78     cp #0x00                                           ;; si no lo está
   4EBF C8            [11]   79     ret z                                              ;; vuelve para no imprimirlo                           
   4EC0 11 00 C0      [10]   80     ld de, #0xC000                                     ;; Inicio de la memoria de video                                
   4EC3 DD 46 01      [19]   81     ld  b, enemiY(ix)                                  ;; Coordenada Y del enemigo en B
   4EC6 DD 4E 00      [19]   82     ld  c, enemiX(ix)                                  ;; Coordenada X del enemigo en C
   4EC9 CD F9 60      [17]   83     call cpct_getScreenPtr_asm
                             84 
   4ECC EB            [ 4]   85     ex de, hl
   4ECD 3E 00         [ 7]   86     ld  a, #0x00                                       ;; El Sprite de la nave 
   4ECF DD BE 08      [19]   87     cp StatusAni(ix)                                   ;; El estatus de la animación 
   4ED2 20 08         [12]   88     jr nz, frameExplo1                                 ;; Si no es el sprite de la nave, ve a explosion 1
   4ED4 DD 6E 04      [19]   89     ld  l, SpriteBajo(ix)
   4ED7 DD 66 05      [19]   90     ld  h, SpriteAlto(ix)                              ;; En HL direccion del Sprite del enemigo
   4EDA 18 3E         [12]   91     jr dibuja                                          ;; Dibuja el sprite de la nave 
   4EDC                      92 frameExplo1:
   4EDC 3C            [ 4]   93     inc  a                                             ;; Ver si el el fotograma 1 de a animación
   4EDD DD BE 08      [19]   94     cp StatusAni(ix)                                   ;; El estatus de la animación
   4EE0 20 08         [12]   95     jr nz, frameExplo2                                 ;; Si no es explosión 1 ver si es explosión 2
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 7.
Hexadecimal [16-Bits]



   4EE2 DD 6E 09      [19]   96     ld  l, frame1Bajo(ix)                              ;; Byte bajo de la dirección del sprite
   4EE5 DD 66 0A      [19]   97     ld  h, frame1Alto(ix)                              ;; Byte alto de la dirección del sprite
   4EE8 18 30         [12]   98     jr dibuja
   4EEA                      99 frameExplo2:
   4EEA 3C            [ 4]  100     inc  a                                             ;; Ver si es el fotograma 2 de la animación
   4EEB DD BE 08      [19]  101     cp StatusAni(ix)                                   ;; El estatus de la animación
   4EEE 20 08         [12]  102     jr nz, frameExplo3                                 ;; Si no es explosión 2 ver si es explosión 3
   4EF0 DD 6E 0B      [19]  103     ld  l, frame2Bajo(ix)                              ;; Byte bajo de la dirección del sprite
   4EF3 DD 66 0C      [19]  104     ld  h, frame2Alto(ix)                              ;; Byte alto de la dirección del sprite
   4EF6 18 22         [12]  105     jr dibuja
   4EF8                     106 frameExplo3:
   4EF8 3C            [ 4]  107     inc  a                                             ;; Ver si es el fotograma 3 de la animación
   4EF9 DD BE 08      [19]  108     cp StatusAni(ix)                                   ;; El estatus de la animación
   4EFC 20 08         [12]  109     jr nz, frameExplo4                                 ;; Si no es la explosión 3 ver si es la 5 
   4EFE DD 6E 0D      [19]  110     ld  l, frame3Bajo(ix)                              ;; Byte bajo de la dirección del sprite 
   4F01 DD 66 0E      [19]  111     ld  h, frame3Alto(ix)                              ;; Byte alto de la dirección del sprite 
   4F04 18 14         [12]  112     jr dibuja
   4F06                     113 frameExplo4:
   4F06 3C            [ 4]  114     inc  a                                             ;; Ver si es el fotograma 4 de la animación 
   4F07 DD BE 08      [19]  115     cp StatusAni(ix)                                   ;; El estatus de la animación   
   4F0A 20 08         [12]  116     jr nz, frameExplo5                                 ;; Si no es la explosión 4 ver si es la 5 
   4F0C DD 6E 0F      [19]  117     ld  l, frame4Bajo(ix)                              ;; Byte bajo de la dirección del sprite 
   4F0F DD 66 10      [19]  118     ld  h, frame4Alto(ix)                              ;; Byte alto de la dirección del sprite 
   4F12 18 06         [12]  119     jr dibuja
   4F14                     120 frameExplo5:
   4F14 DD 6E 11      [19]  121     ld  l, frame5Bajo(ix)                              ;; Byte bajo de la dirección del sprite
   4F17 DD 66 12      [19]  122     ld  h, frame5Alto(ix)                              ;; Byte alto de la dirección del sprite
   4F1A                     123 dibuja:
   4F1A DD 46 02      [19]  124     ld  b, enemiAlto(ix)                               ;; Alto enemigo en B (en bytes)
   4F1D DD 4E 03      [19]  125     ld  c, enemiAncho(ix)                              ;; Ancho enemigo en C (en bytes)
   4F20 CD 48 5E      [17]  126     call cpct_drawSprite_asm                           ;; Borra el último frame de la animación de la explosión
                            127 
   4F23 C9            [10]  128     ret                                                ;; Aquí acaba draeçw_enemy_sprite
                            129 
   4F24                     130 erase_enemy::
   4F24 FD 21 8A 4E   [14]  131     ld iy, #num_enemigos
   4F28 FD 5E 00      [19]  132     ld  e, 0(iy)                                      ;; En E el número de enemigos                       
   4F2B                     133 sigui_enemy:
   4F2B D5            [11]  134     push de
   4F2C CD 3A 4F      [17]  135     call erase_enemy_sprite                           ;; Borrar una entidad enemigo enemigo
   4F2F D1            [10]  136     pop  de   
   4F30 1D            [ 4]  137     dec e                                             ;; Resta uno al total de entidades enemigas
   4F31 C8            [11]  138     ret z                                             ;; Si no quedan enemigos vuelve
   4F32 01 14 00      [10]  139     ld bc, #tamagno_enemy                             ;; El tamaño de los datos de un enemigo
   4F35 DD 09         [15]  140     add ix,bc                                         ;; Se suma a Ix oara deslazar el puntero
   4F37 18 F2         [12]  141     jr sigui_enemy                                    ;; Siguiente enemigo a borrar
   4F39 C9            [10]  142     ret
                            143 
   4F3A                     144 erase_enemy_sprite:
   4F3A DD 7E 13      [19]  145     ld  a, conVida(ix)                                ;; Ver si el enemigo esta vivo
   4F3D FE 00         [ 7]  146     cp #0x00                                          ;; Si no lo esta
   4F3F C8            [11]  147     ret z                                             ;; vuelve 
   4F40 11 00 C0      [10]  148     ld de, #0xC000                                    ;; Inicio de la memoria de video
   4F43 DD 46 01      [19]  149     ld  b, enemiY(ix)                                 ;; Coordenada Y del enemigo
   4F46 DD 4E 00      [19]  150     ld  c, enemiX(ix)                                 ;; Coordenada X del enemigo
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 8.
Hexadecimal [16-Bits]



   4F49 CD F9 60      [17]  151     call cpct_getScreenPtr_asm
                            152 
   4F4C EB            [ 4]  153     ex de,hl                                          ;; Necesario por que la dirección de video debe estar en DE
   4F4D 3E 00         [ 7]  154     ld  a, #0x00                                      ;; Pintar con color cero
   4F4F DD 46 02      [19]  155     ld  b, enemiAlto(ix)                              ;; Alto del Sprite del enemigo en B (en bytes)
   4F52 DD 4E 03      [19]  156     ld  c, enemiAncho(ix)                             ;; Ancho del Sprite del enemigo en C (en bytes)
   4F55 CD 11 60      [17]  157     call cpct_drawSolidBox_asm                        ;; Dibujar una caja con el color del fondo
                            158 
   4F58 C9            [10]  159     ret
                            160 
   4F59                     161 update_enemy::
   4F59 FD 21 8A 4E   [14]  162     ld iy, #num_enemigos
   4F5D FD 5E 00      [19]  163     ld  e, 0(iy)                                      ;; En E el números de enemigos          
   4F60                     164 sigui_update:   
   4F60 D5            [11]  165     push de                                           ;; Preservo E
   4F61 CD 75 4F      [17]  166     call update_spr_enemy                             ;; Actualiza posición X e Y de los enemigos
   4F64 CD 97 4F      [17]  167     call update_tempo_enemy                           ;; Actualiza el tiempo que se ve el fotograma
   4F67 CD DC 4F      [17]  168     call contadorEnemigos                             ;; Actualizar el contador de enemigos aparecidos**************
   4F6A D1            [10]  169     pop  de                                           ;; Recupero número de entidades enemigas
   4F6B 1D            [ 4]  170     dec e                                             ;; Resta uno al total de entidades enemigas
   4F6C C8            [11]  171     ret z                                             ;; Si no quedan enemigos vuelve
   4F6D 01 14 00      [10]  172     ld bc, #tamagno_enemy                             ;; El tamaño de los datos de un enemigo
   4F70 DD 09         [15]  173     add ix,bc                                         ;; Se suma a IX para deslazar el puntero
   4F72 18 EC         [12]  174     jr sigui_update                                   ;; Siguiente enemigo a actualizar posición
   4F74 C9            [10]  175     ret
                            176 
   4F75                     177 update_spr_enemy:
   4F75 DD 7E 01      [19]  178     ld  a, enemiY(ix)                                 ;; En el acumulador la coordenada Y del enemigo
   4F78 DD 46 06      [19]  179     ld  b, enemiVelo(ix)                              ;; El valor de la velocidad del enemigo
   4F7B 80            [ 4]  180     add a, b                                          ;; Se suma resultado en A
   4F7C FE C8         [ 7]  181     cp  #200
   4F7E 28 06         [12]  182     jr z, otroAlien                                   ;; Si A == 0
   4F80 30 04         [12]  183     jr nc, otroAlien                                  ;; || A > 200 que se cree otro alien
   4F82 DD 77 01      [19]  184     ld enemiY(ix), a                                  ;; Se guarda la nueva posición Y del enemigo
   4F85 C9            [10]  185     ret
   4F86                     186 otroAlien:
   4F86 3E 09         [ 7]  187     ld  a, #0x09                                      ;; Reset de la coordenada Y del enemigo
   4F88 DD 77 01      [19]  188     ld  enemiY(ix), a                                 ;; Se guarda
   4F8B CD 8D 4E      [17]  189     call calXenemy                                    ;; Calcula de forma aleatoria otra coordenada X
   4F8E 21 8B 4E      [10]  190     ld hl, #oleada                                    ;; ****** Número total de enemigos dibujados
   4F91 AF            [ 4]  191     xor  a                                            ;; ****** Acumulador a cero
   4F92 BE            [ 7]  192     cp (hl)                                           ;; ****** Ver si ha llegado a cero el número total de enemigos dibujados
   4F93 28 01         [12]  193     jr z, escero                                      ;; ****** Si no es cero
   4F95 35            [11]  194     dec (hl)                                          ;; ****** decrementa
   4F96                     195 escero:
   4F96 C9            [10]  196     ret
                            197 
   4F97                     198 update_tempo_enemy:
   4F97 DD 7E 08      [19]  199     ld  a, StatusAni(ix)                              ;;  Comprobar el estado de la animación
   4F9A FE 00         [ 7]  200     cp #0x00                                          ;;  Si es cero, no esta explotando 
   4F9C C8            [11]  201     ret z                                             ;;  Por lo tanto vuelve 
   4F9D DD 7E 07      [19]  202     ld  a, temporiza(ix)                              ;;  Valor actual del temporizador
   4FA0 FE 02         [ 7]  203     cp  #0x02                                         ;;  Ver si han pasado el número de ciclosa
   4FA2 20 2A         [12]  204     jr nz, masCiclos                                  ;;  Si no ha llegado sigue sumando ciclos
   4FA4 DD 36 07 00   [19]  205     ld  temporiza(ix), #0x00                          ;;  El temporizador a cero
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 9.
Hexadecimal [16-Bits]



   4FA8 DD 34 08      [23]  206     inc StatusAni(ix)                                 ;;  El siguiente fotograma de la animación 
   4FAB DD 7E 08      [19]  207     ld a, StatusAni(ix)                               ;;  El estado de la animación al acumulador
   4FAE FE 05         [ 7]  208     cp #0x05                                          ;;  Ver si es la 6 animación
   4FB0 20 1B         [12]  209     jr nz, vuelve                                     ;;  Si no lo es siguiente animación
   4FB2 11 00 C0      [10]  210     ld de, #0xC000                                    ;;  Inicio de la memoria de video                          
   4FB5 DD 46 01      [19]  211     ld  b, enemiY(ix)                                 ;;  En B la coordenada Y del sprite
   4FB8 3E 09         [ 7]  212     ld  a, #0x09                                      ;;  Reset de la coordenada Y del enemigo
   4FBA DD 77 01      [19]  213     ld  enemiY(ix), a                                 ;;  Se guarda en la coordenada Y del enemigo
   4FBD CD 8D 4E      [17]  214     call calXenemy                                    ;;  Nueva coordenada X aleatoria para el enemigo
   4FC0 CD F9 60      [17]  215     call cpct_getScreenPtr_asm                        ;;  Borra el último sprite de la animación
   4FC3 DD 36 08 00   [19]  216     ld StatusAni(ix), #0x00                           ;;  Vuelta al fotograma cero
                            217     ;;ld hl, #oleada                                    ;;  ******Número total de enemigos que han aparecido
                            218     ;;ld a, (hl)                                        ;;  ******Al acumulador
                            219     ;;cp #0x00                                          ;;  ******Si es cero
                            220     ;;jr z, acaba                                       ;;  ******No aparecen más enemigos *************************
   4FC7 DD 36 06 01   [19]  221     ld enemiVelo(ix), #0x01                           ;;  Activar la velocidad del enemigo
   4FCB 18 00         [12]  222     jr vuelve                                         ;; 
   4FCD                     223 acaba:                                                ;; 
                            224     ;ld conVida(ix), #0x00                             ;;  *****El enemigo esta muerto
                            225     
   4FCD                     226 vuelve:
   4FCD C9            [10]  227     ret
   4FCE                     228 masCiclos:
   4FCE DD 34 07      [23]  229     inc temporiza(ix)                                 ;; Aumenta en uno el temporizador
   4FD1 C9            [10]  230     ret
                            231 
   4FD2                     232 posXenemyPtr::
   4FD2 DD 21 26 4E   [14]  233     ld ix, #enemyX                                     ;; Devuelve en HL la dirección de enemyX  
   4FD6 C9            [10]  234     ret
                            235 
   4FD7                     236 posYenemyPtr::
   4FD7 DD 21 27 4E   [14]  237     ld ix, #enemyY                                    ;; Devuelve en HL la dirección de enemyY
   4FDB C9            [10]  238     ret
                            239 
   4FDC                     240 contadorEnemigos:
   4FDC 21 8B 4E      [10]  241     ld hl, #oleada
   4FDF 7E            [ 7]  242     ld  a, (hl)
   4FE0 FE 00         [ 7]  243     cp #0x00
   4FE2 C0            [11]  244     ret nz
   4FE3 DD 36 13 00   [19]  245     ld conVida(ix), #0x00
   4FE7 C9            [10]  246     ret
