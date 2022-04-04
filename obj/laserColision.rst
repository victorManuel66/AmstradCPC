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



                              4 .include "enemy.h.s"
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



                              6 .include "Player.h.s"
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 6.
Hexadecimal [16-Bits]



                              7 .include "datos.h.s"
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 7.
Hexadecimal [16-Bits]



                              8 .include "destroyEnemy.h.s"
                              1 .globl destruyeEnemigo
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 8.
Hexadecimal [16-Bits]



                              9 .include "marcador.h.s"
                              1 .globl suma
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 9.
Hexadecimal [16-Bits]



                             10 
                             11 ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
                             12 ;; ESTA RUTINA COMPRUEBA SI EL LASER COLISONA CONTRA EL ENEMIGO ;;
                             13 ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
                             14 
   4FF3                      15 laser_colision::
   4FF3 FD 21 8A 4E   [14]   16     ld iy, #num_enemigos
   4FF7 FD 5E 00      [19]   17     ld  e, 0(iy)                                    ;; Cargar E con el contenido de la posición de memoria num_enemigos                        
   4FFA                      18 sigui_enemy:
   4FFA D5            [11]   19     push de                                         ;; Preservo E por que el sonido corrompe DE
   4FFB CD 08 50      [17]   20     call laser_coli                                 ;; Ver si collisiona un enemigo
   4FFE D1            [10]   21     pop  de                                         ;; Recupero número de entidades enemigas
   4FFF 1D            [ 4]   22     dec e                                           ;; Resta uno al total de entidades enemigas
   5000 C8            [11]   23     ret z                                           ;; Si no quedan enemigos vuelve
   5001 01 14 00      [10]   24     ld bc, #tamagno_enemy                           ;; El tamaño de los datos de un enemigo
   5004 DD 09         [15]   25     add ix,bc                                       ;; Se suma a IX para desplazar el puntero
   5006 18 F2         [12]   26     jr sigui_enemy                                  ;; Siguiente enemigo
                             27  
   5008                      28 laser_coli: 
   5008 DD 7E 08      [19]   29     ld  a, StatusAni(ix)                            ;; Comprobar el estado de la animación 
   500B FE 00         [ 7]   30     cp #0x00                                        ;; Si no es cero es que esta explotando 
   500D C0            [11]   31     ret nz                                          ;; Por lo tanto vuelve por que no hay que comprobar colosiones
   500E DD 46 00      [19]   32     ld  b, enemiX(ix)                               ;; En B coordenada X del enemigo                                
   5011 CD 1E 4E      [17]   33     call posXlaserPtr                               ;; HL la dirección de la coordenada Y del laser
   5014 7E            [ 7]   34     ld  a,(hl)                                      ;; el valor al acumulador
   5015 B8            [ 4]   35     cp  b                                           ;; Se comparan
   5016 D8            [11]   36     ret c                                           ;; Si A<B no hay colisión en eje X
   5017 04            [ 4]   37     inc b
   5018 04            [ 4]   38     inc b                                           ;; Suma dos al ancho del enemigo. Esto es mejorable, sumando el ancho de cualquier sprite
   5019 B8            [ 4]   39     cp  b                                           ;; Para ver si A>B
   501A 28 01         [12]   40     jr  z, verY                                     ;; Si son iguales hay colision en X
   501C D0            [11]   41     ret nc                                          ;; Si A>B ho hay colision en el eje X
   501D                      42 verY:
   501D DD 7E 01      [19]   43     ld  a, 1(ix)                                    ;; En A la coordenada Y del enemigo
   5020 C6 08         [ 7]   44     add a, #0x08                                    ;; por que el enemigo tiene 8 bytes de alto, esto también es mejorable
   5022 47            [ 4]   45     ld  b, a                                        ;; ahora en B
   5023 CD 22 4E      [17]   46     call posYlaserPtr                               ;; HL la dirección de la coordenada Y del laser
   5026 7E            [ 7]   47     ld  a, (hl)                                     ;; el valor al acumulador
   5027 B8            [ 4]   48     cp  a,b                                         ;; se comparan
   5028 D0            [11]   49     ret nc                                          ;; Si A>B no hay colisión
                             50 
                             51     ;; Si llegas aquí es que ha habido colision
                             52 
   5029 DD E5         [15]   53     push ix                                         ;; Por que explosion corrompe IX
   502B CD 56 50      [17]   54     call explosion                                  ;; Sonido de explosion
   502E DD E1         [14]   55     pop ix
                             56 
                             57 
                             58     ;; Destruir el laser
   5030 CD 22 4E      [17]   59     call posYlaserPtr                               ;; Posición de memoria coordenada Y del laser
   5033 46            [ 7]   60     ld  b,(hl)                                      ;; Registro B con la posición Y del laser
   5034 3E BD         [ 7]   61     ld  a, #0xBD                                    ;; Coordenada Y del laser reseteada
   5036 77            [ 7]   62     ld (hl), a                                      ;; Coordenada Y del laser ahora hay un nueve
   5037 CD 1E 4E      [17]   63     call posXlaserPtr                               ;; Pedir la posición X de láser
   503A 4E            [ 7]   64     ld c,(hl)                                       ;; Se guarda en C
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 10.
Hexadecimal [16-Bits]



   503B 11 00 C0      [10]   65     ld de, #0xC000                                  ;; DE tiene la posición de inicio de la pantalla
   503E CD 04 61      [17]   66     call cpct_getScreenPtr_asm                      ;; Calcular la posición de memoria
   5041 EB            [ 4]   67     ex de, hl
   5042 3E 00         [ 7]   68     ld  a, #0x00
   5044 01 01 04      [10]   69     ld bc, #0x0401
   5047 CD 1C 60      [17]   70     call cpct_drawSolidBox_asm                      ;; Se dibuja un cuadrado en color del fondo para borrar el láser
   504A CD 18 4A      [17]   71     call disparando                                 ;; Para acceder a la posición de memoria disparando
   504D AF            [ 4]   72     xor a                                           ;; Se pone a cero para indicar que ya no se esta disparando
   504E 77            [ 7]   73     ld (hl),a                                       ;; Y se guarda en disparando
                             74     
                             75     ;; Destruir el alien
   504F CD 2B 4A      [17]   76     call destruyeEnemigo                            ;; Destruye al alien enemigo
   5052 CD 4C 4B      [17]   77     call suma                                       ;; Suma puntos al marcador
                             78     
   5055 C9            [10]   79     ret
                             80 
   5056                      81 explosion:
   5056 3E 02         [ 7]   82     ld  a, #0x02                                    ;; Para el efecto que esta sonando en el canal 2
   5058 CD B0 5D      [17]   83     call cpct_akp_SFXStop_asm
                             84     ;; A = No Channel (0,1,2)
                             85     ;; L = Instrument Number (>0)
                             86     ;; H = Volume (0...F)
                             87     ;; E = Note (0...143)
                             88     ;; D = Speed (0 = As original, 1...255 = new Speed (1 is the fastest))
                             89     ;; BC = Inverted Pitch (-#FFFF -> FFFF). 0 is no pitch. The higher the pitch, the lower the sound.
   505B 3E 01         [ 7]   90     ld  a, #0x01
   505D 21 04 0F      [10]   91     ld hl, #0x0F04
   5060 11 14 00      [10]   92     ld de, #0x0014
   5063 01 00 00      [10]   93     ld bc, #0x0000
   5066 CD 5E 5D      [17]   94     call cpct_akp_SFXPlay_asm
   5069 C9            [10]   95     ret
