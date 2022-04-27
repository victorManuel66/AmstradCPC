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
   4E21                      32 EntEnemigas enemy,  0x00, 0x09, 0x08, 0x03, _enemigo, 0x01, 0x00, 0x00, _explo1, _explo2, _explo3, _explo4, _explo5, 0x01
   0000                       1   enemy::
   4E21 00                    2     enemyX:      .db 0x00                      ;; Coordenada X del enemigo
   4E22 09                    3     enemyY:      .db 0x09                      ;; Coordenada Y del enemigo
   4E23 08                    4     enemyH:      .db 0x08                      ;; Altura del sprite del enemigo
   4E24 03                    5     enemyW:      .db 0x03                      ;; Ancho del sprite del enemigo
   4E25 2E 49                 6     enemyspr:    .dw _enemigo               ;; Dirección del sprite del enemigo
   4E27 01                    7     enemyvel:    .db 0x01                   ;; Velocidad del enemigo
   4E28 00                    8     enemytiemp:  .db 0x00                   ;; Tiempo entre fotográmas
   4E29 00                    9     enemystatus: .db 0x00                    ;; Estado de la animación al explotar
   4E2A 16 49                10     enemyani1:   .dw _explo1                ;; Direccion sprite animación 1
   4E2C FE 48                11     enemyani2:   .dw _explo2                ;; Direccion sprite animación 2 
   4E2E E6 48                12     enemyani3:   .dw _explo3                ;; Direccion sprite animación 3
   4E30 CE 48                13     enemyani4:   .dw _explo4                ;; Direccion sprite animación 4
   4E32 B6 48                14     enemyani5:   .dw _explo5                ;; Direccion sprite animación 5
   4E34 01                   15     enemyvivo:   .db 0x01                  ;; 00 indica que esta muerto, 01 indica que esta viva
   4E35                      33 EntEnemigas enemy2, 0x00, 0x09, 0x08, 0x03, _enemigo, 0x01, 0x00, 0x00, _explo1, _explo2, _explo3, _explo4, _explo5, 0x01
   0014                       1   enemy2::
   4E35 00                    2     enemy2X:      .db 0x00                      ;; Coordenada X del enemigo
   4E36 09                    3     enemy2Y:      .db 0x09                      ;; Coordenada Y del enemigo
   4E37 08                    4     enemy2H:      .db 0x08                      ;; Altura del sprite del enemigo
   4E38 03                    5     enemy2W:      .db 0x03                      ;; Ancho del sprite del enemigo
   4E39 2E 49                 6     enemy2spr:    .dw _enemigo               ;; Dirección del sprite del enemigo
   4E3B 01                    7     enemy2vel:    .db 0x01                   ;; Velocidad del enemigo
   4E3C 00                    8     enemy2tiemp:  .db 0x00                   ;; Tiempo entre fotográmas
   4E3D 00                    9     enemy2status: .db 0x00                    ;; Estado de la animación al explotar
   4E3E 16 49                10     enemy2ani1:   .dw _explo1                ;; Direccion sprite animación 1
   4E40 FE 48                11     enemy2ani2:   .dw _explo2                ;; Direccion sprite animación 2 
   4E42 E6 48                12     enemy2ani3:   .dw _explo3                ;; Direccion sprite animación 3
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 5.
Hexadecimal [16-Bits]



   4E44 CE 48                13     enemy2ani4:   .dw _explo4                ;; Direccion sprite animación 4
   4E46 B6 48                14     enemy2ani5:   .dw _explo5                ;; Direccion sprite animación 5
   4E48 01                   15     enemy2vivo:   .db 0x01                  ;; 00 indica que esta muerto, 01 indica que esta viva
   4E49                      34 EntEnemigas enemy3, 0x00, 0x09, 0x08, 0x03, _enemigo, 0x01, 0x00, 0x00, _explo1, _explo2, _explo3, _explo4, _explo5, 0x01
   0028                       1   enemy3::
   4E49 00                    2     enemy3X:      .db 0x00                      ;; Coordenada X del enemigo
   4E4A 09                    3     enemy3Y:      .db 0x09                      ;; Coordenada Y del enemigo
   4E4B 08                    4     enemy3H:      .db 0x08                      ;; Altura del sprite del enemigo
   4E4C 03                    5     enemy3W:      .db 0x03                      ;; Ancho del sprite del enemigo
   4E4D 2E 49                 6     enemy3spr:    .dw _enemigo               ;; Dirección del sprite del enemigo
   4E4F 01                    7     enemy3vel:    .db 0x01                   ;; Velocidad del enemigo
   4E50 00                    8     enemy3tiemp:  .db 0x00                   ;; Tiempo entre fotográmas
   4E51 00                    9     enemy3status: .db 0x00                    ;; Estado de la animación al explotar
   4E52 16 49                10     enemy3ani1:   .dw _explo1                ;; Direccion sprite animación 1
   4E54 FE 48                11     enemy3ani2:   .dw _explo2                ;; Direccion sprite animación 2 
   4E56 E6 48                12     enemy3ani3:   .dw _explo3                ;; Direccion sprite animación 3
   4E58 CE 48                13     enemy3ani4:   .dw _explo4                ;; Direccion sprite animación 4
   4E5A B6 48                14     enemy3ani5:   .dw _explo5                ;; Direccion sprite animación 5
   4E5C 01                   15     enemy3vivo:   .db 0x01                  ;; 00 indica que esta muerto, 01 indica que esta viva
   4E5D                      35 EntEnemigas enemy4, 0x00, 0x09, 0x08, 0x03, _enemigo, 0x01, 0x00, 0x00, _explo1, _explo2, _explo3, _explo4, _explo5, 0x01
   003C                       1   enemy4::
   4E5D 00                    2     enemy4X:      .db 0x00                      ;; Coordenada X del enemigo
   4E5E 09                    3     enemy4Y:      .db 0x09                      ;; Coordenada Y del enemigo
   4E5F 08                    4     enemy4H:      .db 0x08                      ;; Altura del sprite del enemigo
   4E60 03                    5     enemy4W:      .db 0x03                      ;; Ancho del sprite del enemigo
   4E61 2E 49                 6     enemy4spr:    .dw _enemigo               ;; Dirección del sprite del enemigo
   4E63 01                    7     enemy4vel:    .db 0x01                   ;; Velocidad del enemigo
   4E64 00                    8     enemy4tiemp:  .db 0x00                   ;; Tiempo entre fotográmas
   4E65 00                    9     enemy4status: .db 0x00                    ;; Estado de la animación al explotar
   4E66 16 49                10     enemy4ani1:   .dw _explo1                ;; Direccion sprite animación 1
   4E68 FE 48                11     enemy4ani2:   .dw _explo2                ;; Direccion sprite animación 2 
   4E6A E6 48                12     enemy4ani3:   .dw _explo3                ;; Direccion sprite animación 3
   4E6C CE 48                13     enemy4ani4:   .dw _explo4                ;; Direccion sprite animación 4
   4E6E B6 48                14     enemy4ani5:   .dw _explo5                ;; Direccion sprite animación 5
   4E70 01                   15     enemy4vivo:   .db 0x01                  ;; 00 indica que esta muerto, 01 indica que esta viva
   4E71                      36 EntEnemigas enemy5, 0x00, 0x09, 0x08, 0x03, _enemigo, 0x01, 0x00, 0x00, _explo1, _explo2, _explo3, _explo4, _explo5, 0x01
   0050                       1   enemy5::
   4E71 00                    2     enemy5X:      .db 0x00                      ;; Coordenada X del enemigo
   4E72 09                    3     enemy5Y:      .db 0x09                      ;; Coordenada Y del enemigo
   4E73 08                    4     enemy5H:      .db 0x08                      ;; Altura del sprite del enemigo
   4E74 03                    5     enemy5W:      .db 0x03                      ;; Ancho del sprite del enemigo
   4E75 2E 49                 6     enemy5spr:    .dw _enemigo               ;; Dirección del sprite del enemigo
   4E77 01                    7     enemy5vel:    .db 0x01                   ;; Velocidad del enemigo
   4E78 00                    8     enemy5tiemp:  .db 0x00                   ;; Tiempo entre fotográmas
   4E79 00                    9     enemy5status: .db 0x00                    ;; Estado de la animación al explotar
   4E7A 16 49                10     enemy5ani1:   .dw _explo1                ;; Direccion sprite animación 1
   4E7C FE 48                11     enemy5ani2:   .dw _explo2                ;; Direccion sprite animación 2 
   4E7E E6 48                12     enemy5ani3:   .dw _explo3                ;; Direccion sprite animación 3
   4E80 CE 48                13     enemy5ani4:   .dw _explo4                ;; Direccion sprite animación 4
   4E82 B6 48                14     enemy5ani5:   .dw _explo5                ;; Direccion sprite animación 5
   4E84 01                   15     enemy5vivo:   .db 0x01                  ;; 00 indica que esta muerto, 01 indica que esta viva
                             37 
   4E85 05                   38 num_enemigos::             .db 0x05                           ;; Número de enemigos en pantalla a la vez
   4E86 14                   39 oleada::                   .db 0x14                           ;; Número total de la primera oleada de enemigos
   4E87 00                   40 finOleada::                .db 0x00                           ;; Se mataron todos los enemigos de la oleada
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 6.
Hexadecimal [16-Bits]



                             41  
                             42 
                             43 ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
                             44 ;;          ESTA RUTINA NECESITA EN IX LA DIRECCIÓN DE INICIO DE LOS DATOS DE LA ENTIDAD ENEMIGO            ;;
                             45 ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;                   
   4E88                      46 calXenemy::
   4E88 CD CC 5F      [17]   47     call cpct_getRandom_xsp40_u8_asm                   ;; Devuelve en A un numero pseudo aleatorio de 8 bits
   4E8B FE 05         [ 7]   48     cp #0x05                                           ;; Comparo con el valor mímimo
   4E8D 38 06         [12]   49     jr c, menorCuatro                                  ;; Si es menor que cuatro salta a menorCuatro
   4E8F FE 2F         [ 7]   50     cp #0x2f                                           ;; Comparo con 48 decimal
   4E91 30 06         [12]   51     jr nc, mayor47                                     ;; Si no hay acarreo es que es mayor de 48
   4E93 18 06         [12]   52     jr fin                                             ;; Si llego aquí es porque es mayor que 4 y menor de 48                         
   4E95                      53 menorCuatro:
   4E95 C6 05         [ 7]   54     add a,#0x05
   4E97 18 02         [12]   55     jr fin
   4E99                      56 mayor47:
   4E99 18 ED         [12]   57     jr calXenemy
   4E9B                      58 fin:
   4E9B DD 77 00      [19]   59     ld enemiX(ix), a                                   ;; Guarda la nueva coordenada X para el enemigo
   4E9E C9            [10]   60     ret
                             61 
   4E9F                      62 draw_enemy::
   4E9F FD 21 85 4E   [14]   63     ld  iy, #num_enemigos
   4EA3 FD 5E 00      [19]   64     ld  e, 0(iy)
   4EA6                      65 sigui_enemy_draw:                         
   4EA6 D5            [11]   66     push de                                            ;; draw_enemy_sprite corrompe DE
   4EA7 CD B5 4E      [17]   67     call draw_enemy_sprite                             ;; Dibujar una entidad enemigo
   4EAA D1            [10]   68     pop de                                             ;; Recuperar DE con el total de entidades enemigas restantes
   4EAB 1D            [ 4]   69     dec e                                              ;; Resta uno al total de entidades enemigas
   4EAC C8            [11]   70     ret z                                              ;; Si no quedan enemigos vuelve
   4EAD 01 14 00      [10]   71     ld bc, #tamagno_enemy                              ;; El tamaño de los datos de un enemigo
   4EB0 DD 09         [15]   72     add ix,bc                                          ;; Se suma a Ix oara deslazar el puntero
   4EB2 18 F2         [12]   73     jr sigui_enemy_draw                                ;; Siguiente enemigo a dibujar
   4EB4 C9            [10]   74     ret
                             75 
   4EB5                      76 draw_enemy_sprite:
   4EB5 DD 7E 13      [19]   77     ld  a, conVida(ix)                                 ;; Ver si el enemigo esta vivo
   4EB8 FE 00         [ 7]   78     cp #0x00                                           ;; si no lo está
   4EBA C8            [11]   79     ret z                                              ;; vuelve para no imprimirlo                           
   4EBB 11 00 C0      [10]   80     ld de, #0xC000                                     ;; Inicio de la memoria de video                                
   4EBE DD 46 01      [19]   81     ld  b, enemiY(ix)                                  ;; Coordenada Y del enemigo en B
   4EC1 DD 4E 00      [19]   82     ld  c, enemiX(ix)                                  ;; Coordenada X del enemigo en C
   4EC4 CD E9 60      [17]   83     call cpct_getScreenPtr_asm
                             84 
   4EC7 EB            [ 4]   85     ex de, hl
   4EC8 3E 00         [ 7]   86     ld  a, #0x00                                       ;; El Sprite de la nave 
   4ECA DD BE 08      [19]   87     cp StatusAni(ix)                                   ;; El estatus de la animación 
   4ECD 20 08         [12]   88     jr nz, frameExplo1                                 ;; Si no es el sprite de la nave, ve a explosion 1
   4ECF DD 6E 04      [19]   89     ld  l, SpriteBajo(ix)
   4ED2 DD 66 05      [19]   90     ld  h, SpriteAlto(ix)                              ;; En HL direccion del Sprite del enemigo
   4ED5 18 3E         [12]   91     jr dibuja                                          ;; Dibuja el sprite de la nave 
   4ED7                      92 frameExplo1:
   4ED7 3C            [ 4]   93     inc  a                                             ;; Ver si el el fotograma 1 de a animación
   4ED8 DD BE 08      [19]   94     cp StatusAni(ix)                                   ;; El estatus de la animación
   4EDB 20 08         [12]   95     jr nz, frameExplo2                                 ;; Si no es explosión 1 ver si es explosión 2
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 7.
Hexadecimal [16-Bits]



   4EDD DD 6E 09      [19]   96     ld  l, frame1Bajo(ix)                              ;; Byte bajo de la dirección del sprite
   4EE0 DD 66 0A      [19]   97     ld  h, frame1Alto(ix)                              ;; Byte alto de la dirección del sprite
   4EE3 18 30         [12]   98     jr dibuja
   4EE5                      99 frameExplo2:
   4EE5 3C            [ 4]  100     inc  a                                             ;; Ver si es el fotograma 2 de la animación
   4EE6 DD BE 08      [19]  101     cp StatusAni(ix)                                   ;; El estatus de la animación
   4EE9 20 08         [12]  102     jr nz, frameExplo3                                 ;; Si no es explosión 2 ver si es explosión 3
   4EEB DD 6E 0B      [19]  103     ld  l, frame2Bajo(ix)                              ;; Byte bajo de la dirección del sprite
   4EEE DD 66 0C      [19]  104     ld  h, frame2Alto(ix)                              ;; Byte alto de la dirección del sprite
   4EF1 18 22         [12]  105     jr dibuja
   4EF3                     106 frameExplo3:
   4EF3 3C            [ 4]  107     inc  a                                             ;; Ver si es el fotograma 3 de la animación
   4EF4 DD BE 08      [19]  108     cp StatusAni(ix)                                   ;; El estatus de la animación
   4EF7 20 08         [12]  109     jr nz, frameExplo4                                 ;; Si no es la explosión 3 ver si es la 5 
   4EF9 DD 6E 0D      [19]  110     ld  l, frame3Bajo(ix)                              ;; Byte bajo de la dirección del sprite 
   4EFC DD 66 0E      [19]  111     ld  h, frame3Alto(ix)                              ;; Byte alto de la dirección del sprite 
   4EFF 18 14         [12]  112     jr dibuja
   4F01                     113 frameExplo4:
   4F01 3C            [ 4]  114     inc  a                                             ;; Ver si es el fotograma 4 de la animación 
   4F02 DD BE 08      [19]  115     cp StatusAni(ix)                                   ;; El estatus de la animación   
   4F05 20 08         [12]  116     jr nz, frameExplo5                                 ;; Si no es la explosión 4 ver si es la 5 
   4F07 DD 6E 0F      [19]  117     ld  l, frame4Bajo(ix)                              ;; Byte bajo de la dirección del sprite 
   4F0A DD 66 10      [19]  118     ld  h, frame4Alto(ix)                              ;; Byte alto de la dirección del sprite 
   4F0D 18 06         [12]  119     jr dibuja
   4F0F                     120 frameExplo5:
   4F0F DD 6E 11      [19]  121     ld  l, frame5Bajo(ix)                              ;; Byte bajo de la dirección del sprite
   4F12 DD 66 12      [19]  122     ld  h, frame5Alto(ix)                              ;; Byte alto de la dirección del sprite
   4F15                     123 dibuja:
   4F15 DD 46 02      [19]  124     ld  b, enemiAlto(ix)                               ;; Alto enemigo en B (en bytes)
   4F18 DD 4E 03      [19]  125     ld  c, enemiAncho(ix)                              ;; Ancho enemigo en C (en bytes)
   4F1B CD 38 5E      [17]  126     call cpct_drawSprite_asm                           ;; Borra el último frame de la animación de la explosión
                            127 
   4F1E C9            [10]  128     ret                                                ;; Aquí acaba draeçw_enemy_sprite
                            129 
   4F1F                     130 erase_enemy::
   4F1F FD 21 85 4E   [14]  131     ld iy, #num_enemigos
   4F23 FD 5E 00      [19]  132     ld  e, 0(iy)                                      ;; En E el número de enemigos                       
   4F26                     133 sigui_enemy:
   4F26 D5            [11]  134     push de
   4F27 CD 35 4F      [17]  135     call erase_enemy_sprite                           ;; Borrar una entidad enemigo enemigo
   4F2A D1            [10]  136     pop  de   
   4F2B 1D            [ 4]  137     dec e                                             ;; Resta uno al total de entidades enemigas
   4F2C C8            [11]  138     ret z                                             ;; Si no quedan enemigos vuelve
   4F2D 01 14 00      [10]  139     ld bc, #tamagno_enemy                             ;; El tamaño de los datos de un enemigo
   4F30 DD 09         [15]  140     add ix,bc                                         ;; Se suma a Ix oara deslazar el puntero
   4F32 18 F2         [12]  141     jr sigui_enemy                                    ;; Siguiente enemigo a borrar
   4F34 C9            [10]  142     ret
                            143 
   4F35                     144 erase_enemy_sprite:
   4F35 DD 7E 13      [19]  145     ld  a, conVida(ix)                                ;; Ver si el enemigo esta vivo
   4F38 FE 00         [ 7]  146     cp #0x00                                          ;; Si no lo esta
   4F3A C8            [11]  147     ret z                                             ;; vuelve 
   4F3B 11 00 C0      [10]  148     ld de, #0xC000                                    ;; Inicio de la memoria de video
   4F3E DD 46 01      [19]  149     ld  b, enemiY(ix)                                 ;; Coordenada Y del enemigo
   4F41 DD 4E 00      [19]  150     ld  c, enemiX(ix)                                 ;; Coordenada X del enemigo
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 8.
Hexadecimal [16-Bits]



   4F44 CD E9 60      [17]  151     call cpct_getScreenPtr_asm
                            152 
   4F47 EB            [ 4]  153     ex de,hl                                          ;; Necesario por que la dirección de video debe estar en DE
   4F48 3E 00         [ 7]  154     ld  a, #0x00                                      ;; Pintar con color cero
   4F4A DD 46 02      [19]  155     ld  b, enemiAlto(ix)                              ;; Alto del Sprite del enemigo en B (en bytes)
   4F4D DD 4E 03      [19]  156     ld  c, enemiAncho(ix)                             ;; Ancho del Sprite del enemigo en C (en bytes)
   4F50 CD 01 60      [17]  157     call cpct_drawSolidBox_asm                        ;; Dibujar una caja con el color del fondo
                            158 
   4F53 C9            [10]  159     ret
                            160 
   4F54                     161 update_enemy::
   4F54 FD 21 85 4E   [14]  162     ld iy, #num_enemigos
   4F58 FD 5E 00      [19]  163     ld  e, 0(iy)                                      ;; En E el números de enemigos          
   4F5B                     164 sigui_update:   
   4F5B D5            [11]  165     push de                                           ;; Preservo E
   4F5C CD 6D 4F      [17]  166     call update_spr_enemy                             ;; Actualiza posición X e Y de los enemigos
   4F5F CD 96 4F      [17]  167     call update_tempo_enemy                           ;; Actualiza el tiempo que se ve el fotograma
   4F62 D1            [10]  168     pop  de                                           ;; Recupero número de entidades enemigas
   4F63 1D            [ 4]  169     dec e                                             ;; Resta uno al total de entidades enemigas
   4F64 C8            [11]  170     ret z                                             ;; Si no quedan enemigos vuelve
   4F65 01 14 00      [10]  171     ld bc, #tamagno_enemy                             ;; El tamaño de los datos de un enemigo
   4F68 DD 09         [15]  172     add ix,bc                                         ;; Se suma a IX para deslazar el puntero
   4F6A 18 EF         [12]  173     jr sigui_update                                   ;; Siguiente enemigo a actualizar posición
   4F6C C9            [10]  174     ret
                            175 
   4F6D                     176 update_spr_enemy:
   4F6D DD 7E 01      [19]  177     ld  a, enemiY(ix)                                 ;; En el acumulador la coordenada Y del enemigo
   4F70 DD 46 06      [19]  178     ld  b, enemiVelo(ix)                              ;; El valor de la velocidad del enemigo
   4F73 80            [ 4]  179     add a, b                                          ;; Se suma resultado en A
   4F74 FE C8         [ 7]  180     cp  #200
   4F76 28 06         [12]  181     jr z, otroAlien                                   ;; Si A == 0
   4F78 30 04         [12]  182     jr nc, otroAlien                                  ;; || A > 200 que se cree otro alien
   4F7A DD 77 01      [19]  183     ld enemiY(ix), a                                  ;; Se guarda la nueva posición Y del enemigo
   4F7D C9            [10]  184     ret
   4F7E                     185 otroAlien:
   4F7E 3A 87 4E      [13]  186     ld  a, (finOleada)                                ;; ***** Ver si el final de la oleada ****
   4F81 FE 01         [ 7]  187     cp  #0x01                                         ;; ***************************************
   4F83 28 10         [12]  188     jr  z, final                                      ;; ***** Si esta a uno no dibujes más enemigos **********
   4F85 3E 09         [ 7]  189     ld  a, #0x09                                      ;; Reset de la coordenada Y del enemigo
   4F87 DD 77 01      [19]  190     ld  enemiY(ix), a                                 ;; Se guarda
   4F8A CD 88 4E      [17]  191     call calXenemy                                    ;; Calcula de forma aleatoria otra coordenada X
   4F8D 21 86 4E      [10]  192     ld hl, #oleada                                    ;; ****** Número total de enemigos dibujados
   4F90 AF            [ 4]  193     xor  a                                            ;; ****** Acumulador a cero
   4F91 BE            [ 7]  194     cp (hl)                                           ;; ****** Ver si ha llegado a cero el número total de enemigos dibujados
   4F92 28 01         [12]  195     jr z, final                                       ;; ****** Si no es cero
   4F94 35            [11]  196     dec (hl)                                          ;; ****** decrementa
   4F95                     197 final:
   4F95 C9            [10]  198     ret
                            199 
   4F96                     200 update_tempo_enemy:
   4F96 DD 7E 08      [19]  201     ld  a, StatusAni(ix)                              ;;  Comprobar el estado de la animación
   4F99 FE 00         [ 7]  202     cp #0x00                                          ;;  Si es cero, no esta explotando 
   4F9B C8            [11]  203     ret z                                             ;;  Por lo tanto vuelve 
   4F9C DD 7E 07      [19]  204     ld  a, temporiza(ix)                              ;;  Valor actual del temporizador
   4F9F FE 02         [ 7]  205     cp  #0x02                                         ;;  Ver si han pasado el número de ciclosa
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 9.
Hexadecimal [16-Bits]



   4FA1 20 28         [12]  206     jr nz, masCiclos                                  ;;  Si no ha llegado sigue sumando ciclos
   4FA3 DD 36 07 00   [19]  207     ld  temporiza(ix), #0x00                          ;;  El temporizador a cero
   4FA7 DD 34 08      [23]  208     inc StatusAni(ix)                                 ;;  El siguiente fotograma de la animación 
   4FAA DD 7E 08      [19]  209     ld a, StatusAni(ix)                               ;;  El estado de la animación al acumulador
   4FAD FE 05         [ 7]  210     cp #0x05                                          ;;  Ver si es la 6 animación
   4FAF 20 19         [12]  211     jr nz, vuelve                                     ;;  Si no lo es siguiente animación
   4FB1 11 00 C0      [10]  212     ld de, #0xC000                                    ;;  Inicio de la memoria de video                          
   4FB4 DD 46 01      [19]  213     ld  b, enemiY(ix)                                 ;;  En B la coordenada Y del sprite
   4FB7 3E 09         [ 7]  214     ld  a, #0x09                                      ;;  Reset de la coordenada Y del enemigo
   4FB9 DD 77 01      [19]  215     ld  enemiY(ix), a                                 ;;  Se guarda en la coordenada Y del enemigo
   4FBC CD 88 4E      [17]  216     call calXenemy                                    ;;  Nueva coordenada X aleatoria para el enemigo
   4FBF CD E9 60      [17]  217     call cpct_getScreenPtr_asm                        ;;  Borra el último sprite de la animación
   4FC2 DD 36 08 00   [19]  218     ld StatusAni(ix), #0x00                           ;;  Vuelta al fotograma cero
   4FC6 DD 36 06 01   [19]  219     ld enemiVelo(ix), #0x01                           ;;  Activar la velocidad del enemigo
                            220     ;jr vuelve                                         ;; 
                            221 
   4FCA                     222 vuelve:
   4FCA C9            [10]  223     ret
   4FCB                     224 masCiclos:
   4FCB DD 34 07      [23]  225     inc temporiza(ix)                                 ;; Aumenta en uno el temporizador
   4FCE C9            [10]  226     ret
                            227 
   4FCF                     228 posXenemyPtr::
   4FCF DD 21 21 4E   [14]  229     ld ix, #enemyX                                     ;; Devuelve en HL la dirección de enemyX  
   4FD3 C9            [10]  230     ret
                            231 
   4FD4                     232 posYenemyPtr::
   4FD4 DD 21 22 4E   [14]  233     ld ix, #enemyY                                    ;; Devuelve en HL la dirección de enemyY
   4FD8 C9            [10]  234     ret
                            235 
