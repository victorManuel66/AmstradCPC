ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 1.
Hexadecimal [16-Bits]



                              1 .include "enemy.h.s"
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 2.
Hexadecimal [16-Bits]



                              2 .include "datos.h.s"
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



                              3 .include "video/video_macros.h.s"
                              1 ;;-----------------------------LICENSE NOTICE------------------------------------
                              2 ;;  This file is part of CPCtelera: An Amstrad CPC Game Engine
                              3 ;;  Copyright (C) 2017 ronaldo / Fremos / Cheesetea / ByteRealms (@FranGallegoBR)
                              4 ;;
                              5 ;;  This program is free software: you can redistribute it and/or modify
                              6 ;;  it under the terms of the GNU Lesser General Public License as published by
                              7 ;;  the Free Software Foundation, either version 3 of the License, or
                              8 ;;  (at your option) any later version.
                              9 ;;
                             10 ;;  This program is distributed in the hope that it will be useful,
                             11 ;;  but WITHOUT ANY WARRANTY; without even the implied warranty of
                             12 ;;  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
                             13 ;;  GNU Lesser General Public License for more details.
                             14 ;;
                             15 ;;  You should have received a copy of the GNU Lesser General Public License
                             16 ;;  along with this program.  If not, see <http://www.gnu.org/licenses/>.
                             17 ;;-------------------------------------------------------------------------------
                             18 
                             19 ;;//////////////////////////////////////////////////////////////////////
                             20 ;;//////////////////////////////////////////////////////////////////////
                             21 ;; File: Macros (asm)
                             22 ;;//////////////////////////////////////////////////////////////////////
                             23 ;;//////////////////////////////////////////////////////////////////////
                             24 
                             25 ;;//////////////////////////////////////////////////////////////////////
                             26 ;; Group: Video memory manipulation
                             27 ;;//////////////////////////////////////////////////////////////////////
                             28 
                             29 ;;
                             30 ;; Constant: CPCT_VMEM_START_ASM
                             31 ;;
                             32 ;;    The address where screen video memory starts by default in the Amstrad CPC.
                             33 ;;
                             34 ;;    This address is exactly 0xC000, and this macro represents this number but
                             35 ;; automatically converted to <u8>* (Pointer to unsigned byte). You can use this
                             36 ;; macro for any function requiring the start of video memory, like 
                             37 ;; <cpct_getScreenPtr>.
                             38 ;;
                     C000    39 CPCT_VMEM_START_ASM = 0xC000
                             40 
                             41 ;;
                             42 ;; Constants: Video Memory Pages
                             43 ;;
                             44 ;; Useful constants defining some typical Video Memory Pages to be used as 
                             45 ;; parameters for <cpct_setVideoMemoryPage>
                             46 ;;
                             47 ;; cpct_pageCO - Video Memory Page 0xC0 (0xC0··)
                             48 ;; cpct_page8O - Video Memory Page 0x80 (0x80··)
                             49 ;; cpct_page4O - Video Memory Page 0x40 (0x40··)
                             50 ;; cpct_page0O - Video Memory Page 0x00 (0x00··)
                             51 ;;
                     0030    52 cpct_pageC0_asm = 0x30
                     0020    53 cpct_page80_asm = 0x20
                     0010    54 cpct_page40_asm = 0x10
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 4.
Hexadecimal [16-Bits]



                     0000    55 cpct_page00_asm = 0x00
                             56 
                             57 ;;
                             58 ;; Macro: cpctm_memPage6_asm
                             59 ;;
                             60 ;;    Macro that encodes a video memory page in the 6 Least Significant bits (LSb)
                             61 ;; of a byte, required as parameter for <cpct_setVideoMemoryPage>. It loads resulting
                             62 ;; value into a given 8-bits register.
                             63 ;;
                             64 ;; ASM Definition:
                             65 ;; .macro <cpct_memPage6_asm> *REG8*, *PAGE*
                             66 ;;
                             67 ;; Parameters (1 byte):
                             68 ;; (__) REG8 - 8bits register where result will be loaded
                             69 ;; (1B) PAGE - Video memory page wanted 
                             70 ;;
                             71 ;; Known issues:
                             72 ;;   * This macro can only be used from assembler code. It is not accessible from 
                             73 ;; C scope. For C programs, please refer to <cpct_memPage6>
                             74 ;;   * This macro will work *only* with constant values, as its value needs to
                             75 ;; be calculated in compilation time. If fed with variable values, it will give 
                             76 ;; an assembler error.
                             77 ;;
                             78 ;; Destroyed Registers:
                             79 ;;    REG8
                             80 ;;
                             81 ;; Size of generated code:
                             82 ;;    2 bytes 
                             83 ;;
                             84 ;; Time Measures:
                             85 ;;    * 2 microseconds
                             86 ;;    * 8 CPU Cycles
                             87 ;;
                             88 ;; Details:
                             89 ;;  This is just a macro that shifts *PAGE* 2 bits to the right, to leave it
                             90 ;; with just 6 significant bits. For more information, check functions
                             91 ;; <cpct_setVideoMemoryPage> and <cpct_setVideoMemoryOffset>.
                             92 ;;
                             93 .macro cpctm_memPage6_asm REG8, PAGE 
                             94    ld REG8, #PAGE / 4      ;; [2] REG8 = PAGE/4
                             95 .endm
                             96 
                             97 ;;
                             98 ;; Macro: cpctm_screenPtr_asm
                             99 ;;
                            100 ;;    Macro that calculates the video memory location (byte pointer) of a 
                            101 ;; given pair of coordinates (*X*, *Y*). Value resulting from calculation 
                            102 ;; will be loaded into a 16-bits register.
                            103 ;;
                            104 ;; ASM Definition:
                            105 ;;    .macro <cpctm_screenPtr_asm> *REG16*, *VMEM*, *X*, *Y*
                            106 ;;
                            107 ;; Parameters:
                            108 ;;    (__) REG16 - 16-bits register where the resulting value will be loaded
                            109 ;;    (2B) VMEM  - Start of video memory buffer where (*X*, *Y*) coordinates will be calculated
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 5.
Hexadecimal [16-Bits]



                            110 ;;    (1B) X     - X Coordinate of the video memory location *in bytes* (*BEWARE! NOT in pixels!*)
                            111 ;;    (1B) Y     - Y Coordinate of the video memory location in pixels / bytes (they are same amount)
                            112 ;;
                            113 ;; Parameter Restrictions:
                            114 ;;    * *REG16* has to be a 16-bits register that can perform ld REG16, #value.
                            115 ;;    * *VMEM* will normally be the start of the video memory buffer where you want to 
                            116 ;; draw something. It could theoretically be any 16-bits value. 
                            117 ;;    * *X* must be in the range [0-79] for normal screen sizes (modes 0,1,2). Screen is
                            118 ;; always 80 bytes wide in these modes and this function is byte-aligned, so you have to 
                            119 ;; give it a byte coordinate (*NOT a pixel one!*).
                            120 ;;    * *Y* must be in the range [0-199] for normal screen sizes (modes 0,1,2). Screen is 
                            121 ;; always 200 pixels high in these modes. Pixels and bytes always coincide in vertical
                            122 ;; resolution, so this coordinate is the same in bytes that in pixels.
                            123 ;;    * If you give incorrect values to this function, the returned pointer could
                            124 ;; point anywhere in memory. This function will not cause any damage by itself, 
                            125 ;; but you may destroy important parts of your memory if you use its result to 
                            126 ;; write to memory, and you gave incorrect parameters by mistake. Take always
                            127 ;; care.
                            128 ;;
                            129 ;; Known issues:
                            130 ;;   * This macro can only be used from assembler code. It is not accessible from 
                            131 ;; C scope. For C programs, please refer to <cpct_getScreenPtr>
                            132 ;;   * This macro will work *only* with constant values, as calculations need to be 
                            133 ;; performed at assembler time.
                            134 ;;
                            135 ;; Destroyed Registers:
                            136 ;;    REG16
                            137 ;;
                            138 ;; Size of generated code:
                            139 ;;    3 bytes 
                            140 ;;
                            141 ;; Time Measures:
                            142 ;;    * 3 microseconds
                            143 ;;    * 12 CPU Cycles
                            144 ;;
                            145 ;; Details:
                            146 ;;    This macro does the same calculation than the function <cpct_getScreenPtr>. However,
                            147 ;; as it is a macro, if all 3 parameters (*VMEM*, *X*, *Y*) are constants, the calculation
                            148 ;; will be done at compile-time. This will free the binary from code or data, just putting in
                            149 ;; the result of this calculation (2 bytes with the resulting address). It is highly 
                            150 ;; recommended to use this macro instead of the function <cpct_getScreenPtr> when values
                            151 ;; involved are all constant. 
                            152 ;;
                            153 ;; Recommendations:
                            154 ;;    All constant values - Use this macro <cpctm_screenPtr_asm>
                            155 ;;    Any variable value  - Use the function <cpct_getScreenPtr>
                            156 ;;
                            157 .macro cpctm_screenPtr_asm REG16, VMEM, X, Y 
                            158    ld REG16, #VMEM + 80 * (Y / 8) + 2048 * (Y & 7) + X   ;; [3] REG16 = screenPtr
                            159 .endm
                            160 
                            161 ;;
                            162 ;; Macro: cpctm_screenPtrSym_asm
                            163 ;;
                            164 ;;    Macro that calculates the video memory location (byte pointer) of a 
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 6.
Hexadecimal [16-Bits]



                            165 ;; given pair of coordinates (*X*, *Y*). Value resulting from calculation 
                            166 ;; will be assigned to the given ASZ80 local symbol.
                            167 ;;
                            168 ;; ASM Definition:
                            169 ;;    .macro <cpctm_screenPtr_asm> *SYM*, *VMEM*, *X*, *Y*
                            170 ;;
                            171 ;; Parameters:
                            172 ;;    (__) SYM   - ASZ80 local symbol to assign the result from the calculation to
                            173 ;;    (2B) VMEM  - Start of video memory buffer where (*X*, *Y*) coordinates will be calculated
                            174 ;;    (1B) X     - X Coordinate of the video memory location *in bytes* (*BEWARE! NOT in pixels!*)
                            175 ;;    (1B) Y     - Y Coordinate of the video memory location in pixels / bytes (they are same amount)
                            176 ;;
                            177 ;; Parameter Restrictions:
                            178 ;;    * *SYM* need to be a valid symbol according to ASZ80 rules for symbols
                            179 ;;    * *VMEM* will normally be the start of the video memory buffer where you want to 
                            180 ;; draw something. It could theoretically be any 16-bits value. 
                            181 ;;    * *X* must be in the range [0-79] for normal screen sizes (modes 0,1,2). Screen is
                            182 ;; always 80 bytes wide in these modes and this function is byte-aligned, so you have to 
                            183 ;; give it a byte coordinate (*NOT a pixel one!*).
                            184 ;;    * *Y* must be in the range [0-199] for normal screen sizes (modes 0,1,2). Screen is 
                            185 ;; always 200 pixels high in these modes. Pixels and bytes always coincide in vertical
                            186 ;; resolution, so this coordinate is the same in bytes that in pixels.
                            187 ;;    * If you give incorrect values to this function, the returned pointer could
                            188 ;; point anywhere in memory. This function will not cause any damage by itself, 
                            189 ;; but you may destroy important parts of your memory if you use its result to 
                            190 ;; write to memory, and you gave incorrect parameters by mistake. Take always
                            191 ;; care.
                            192 ;;
                            193 ;; Known issues:
                            194 ;;   * This macro can only be used from assembler code. It is not accessible from 
                            195 ;; C scope. For C programs, please refer to <cpct_getScreenPtr>
                            196 ;;   * This macro will work *only* with constant values, as calculations need to be 
                            197 ;; performed at assembler time.
                            198 ;;
                            199 ;; Destroyed Registers:
                            200 ;;    none
                            201 ;;
                            202 ;; Size of generated code:
                            203 ;;    none (symbols are compile-time, do not generate code)
                            204 ;;
                            205 ;; Time Measures:
                            206 ;;    - not applicable -
                            207 ;;
                            208 ;; Details:
                            209 ;;    This macro does the same calculation than the function <cpct_getScreenPtr>. However,
                            210 ;; as it is a macro, and as its parameters (*VMEM*, *X*, *Y*) must be constants, the calculation
                            211 ;; will be performed at compile-time. This will free the binary from code or data, just putting in
                            212 ;; the result of this calculation (2 bytes with the resulting address). It is highly 
                            213 ;; recommended to use this macro instead of the function <cpct_getScreenPtr> when values
                            214 ;; involved are all constant. 
                            215 ;;
                            216 ;; Recommendations:
                            217 ;;    All constant values - Use this macro <cpctm_screenPtrSym_asm> or <cpctm_screenPtr_asm> 
                            218 ;;    Any variable value  - Use the function <cpct_getScreenPtr>
                            219 ;;
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 7.
Hexadecimal [16-Bits]



                            220 .macro cpctm_screenPtrSym_asm SYM, VMEM, X, Y 
                            221    SYM = #VMEM + 80 * (Y / 8) + 2048 * (Y & 7) + X 
                            222 .endm
                            223 
                            224 ;;
                            225 ;; Macro: cpctm_setCRTCReg
                            226 ;;
                            227 ;;    Macro that sets a new value for a given CRTC register.
                            228 ;;
                            229 ;; ASM Definition:
                            230 ;;    .macro <cpctm_setCRTCReg> *HEXREG*, *HEXVAL*
                            231 ;;
                            232 ;; Parameters:
                            233 ;;    (1B) HEXREG - New value to be set for the register (in hexadecimal)
                            234 ;;    (1B) HEXVAL - Number of the register to be set (in hexadecimal)
                            235 ;;
                            236 ;; Parameter Restrictions:
                            237 ;;    * *HEXREG* has to be an hexadecimal value from 00 to 1F
                            238 ;;    * *HEXVAL* has to be an hexadecimal value. Its valid range will depend
                            239 ;;          upon the selected register that will be modified. 
                            240 ;;
                            241 ;; Known issues:
                            242 ;;   * This macro can *only* be used from assembler code. It is not accessible from 
                            243 ;; C scope. 
                            244 ;;   * This macro can only be used with *constant values*. As given values are 
                            245 ;; concatenated with a number, they must also be hexadecimal numbers. If a 
                            246 ;; register or other value is given, this macro will not work.
                            247 ;;   * Using values out of range have unpredicted behaviour and can even 
                            248 ;; potentially cause damage to real Amstrad CPC monitors. Please, use with care.
                            249 ;;
                            250 ;; Destroyed Registers:
                            251 ;;    BC
                            252 ;;
                            253 ;; Size of generated code:
                            254 ;;    10 bytes 
                            255 ;;
                            256 ;; Time Measures:
                            257 ;;    * 14 microseconds
                            258 ;;    * 56 CPU Cycles
                            259 ;;
                            260 ;; Details:
                            261 ;;    This macro expands to two CRTC commands: Register selection and Register setting.
                            262 ;; It selects the register given as first parameter, then sets its new value to 
                            263 ;; that given as second parameter. Both given parameters must be of exactly 1 byte
                            264 ;; in size and the have to be provided in hexadecimal. This is due to the way
                            265 ;; that macro expansion and concatenation works. Given values will be concatenated
                            266 ;; with another 8-bit hexadecimal value to form a unique 16-bits hexadecimal value.
                            267 ;; Therefore, any parameter given will always be considered hexadecimal.
                            268 ;;
                            269 .macro cpctm_setCRTCReg_asm HEXREG, HEXVAL
                            270    ld    bc, #0xBC'HEXREG  ;; [3] B=0xBC CRTC Select Register, C=register number to be selected
                            271    out  (c), c             ;; [4] Select register
                            272    ld    bc, #0xBD'HEXVAL  ;; [3] B=0xBD CRTC Set Register, C=Value to be set
                            273    out  (c), c             ;; [4] Set the value
                            274 .endm
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 8.
Hexadecimal [16-Bits]



                            275 
                            276 ;;//////////////////////////////////////////////////////////////////////
                            277 ;; Group: Setting the border
                            278 ;;//////////////////////////////////////////////////////////////////////
                            279 
                            280 ;;
                            281 ;; Macro: cpctm_setBorder_asm
                            282 ;;
                            283 ;;   Changes the colour of the screen border.
                            284 ;;
                            285 ;; ASM Definition:
                            286 ;;   .macro <cpctm_setBorder_asm> HWC 
                            287 ;;
                            288 ;; Input Parameters (1 Byte):
                            289 ;;   (1B) HWC - Hardware colour value for the screen border in *hexadecimal [00-1B]*.
                            290 ;;
                            291 ;; Known issues:
                            292 ;;   * *Beware!* *HWC* colour value must be given in *hexadecimal*, as it is
                            293 ;; substituted in place, and must be in the range [00-1B].
                            294 ;;   * This macro can only be used from assembler code. It is not accessible from 
                            295 ;; C scope. For C programs, please refer to <cpct_setBorder>
                            296 ;;   * This macro will work *only* with constant values, as calculations need to be 
                            297 ;; performed at assembler time.
                            298 ;;
                            299 ;; Destroyed Registers:
                            300 ;;    AF, B, HL
                            301 ;;
                            302 ;; Size of generated code:
                            303 ;;    * 16 bytes 
                            304 ;;     6b - generated code
                            305 ;;    10b - cpct_setPALColour_asm code
                            306 ;;
                            307 ;; Time Measures:
                            308 ;;    * 28 microseconds
                            309 ;;    * 112 CPU Cycles
                            310 ;;
                            311 ;; Details:
                            312 ;;   This is not a real function, but an assembler macro. Beware of using it along
                            313 ;; with complex expressions or calculations, as it may expand in non-desired
                            314 ;; ways.
                            315 ;;
                            316 ;;   For more information, check the real function <cpct_setPALColour>, which
                            317 ;; is called when using <cpctm_setBorder_asm> (It is called using 16 as *pen*
                            318 ;; argument, which identifies the border).
                            319 ;;
                            320 .macro cpctm_setBorder_asm HWC
                            321    .radix h
                            322    cpctm_setBorder_raw_asm \HWC ;; [28] Macro that does the job, but requires a number value to be passed
                            323    .radix d
                            324 .endm
                            325 .macro cpctm_setBorder_raw_asm HWC
                            326    .globl cpct_setPALColour_asm
                            327    ld   hl, #0x'HWC'10         ;; [3]  H=Hardware value of desired colour, L=Border INK (16)
                            328    call cpct_setPALColour_asm  ;; [25] Set Palette colour of the border
                            329 .endm
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 9.
Hexadecimal [16-Bits]



                            330 
                            331 ;;//////////////////////////////////////////////////////////////////////
                            332 ;; Group: Screen clearing
                            333 ;;//////////////////////////////////////////////////////////////////////
                            334 
                            335 ;;
                            336 ;; Macro: cpctm_clearScreen_asm
                            337 ;;
                            338 ;;    Macro to simplify clearing the screen.
                            339 ;;
                            340 ;; ASM Definition:
                            341 ;;   .macro <cpctm_clearScreen_asm> COL
                            342 ;;
                            343 ;; Input Parameters (1 byte):
                            344 ;;   (1B) COL - Colour pattern to be used for screen clearing. 
                            345 ;;
                            346 ;; Parameters:
                            347 ;;    *COL* - Any 8-bits value or the A register are valid. Typically, a 0x00 is used 
                            348 ;; to fill up all the screen with 0's (firmware colour 0). However, you may use it in 
                            349 ;; combination with <cpct_px2byteM0>, <cpct_px2byteM1> or a manually created colour pattern.
                            350 ;;
                            351 ;; Known issues:
                            352 ;;   * This macro can only be used from assembler code. It is not accessible from 
                            353 ;; C scope. For C programs, please refer to <cpct_clearScreen>
                            354 ;;
                            355 ;; Details:
                            356 ;;   Fills up all the standard screen (range [0xC000-0xFFFF]) with *COL* byte, the colour 
                            357 ;; pattern given.
                            358 ;;
                            359 ;; Destroyed Registers:
                            360 ;;    BC, DE, HL
                            361 ;;
                            362 ;; Size of generated code:
                            363 ;;    13 bytes 
                            364 ;;
                            365 ;; Time Measures:
                            366 ;;    98309 microseconds (*4.924 VSYNCs* on a 50Hz display).
                            367 ;;    393236 CPU Cycles 
                            368 ;;
                            369 .macro cpctm_clearScreen_asm COL
                            370    ld    hl, #0xC000    ;; [3] HL Points to Start of Video Memory
                            371    ld    de, #0xC001    ;; [3] DE Points to the next byte
                            372    ld    bc, #(0x4000-1);; [3] BC = 16383 bytes to be copied
                            373    ld   (hl), #COL      ;; [3] First Byte = given Colour
                            374    ldir                 ;; [98297] Perform the copy
                            375 .endm
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 10.
Hexadecimal [16-Bits]



                              4 .include "video/colours.h.s"
                              1 ;;-----------------------------LICENSE NOTICE------------------------------------
                              2 ;;  This file is part of CPCtelera: An Amstrad CPC Game Engine
                              3 ;;  Copyright (C) 2017 ronaldo / Fremos / Cheesetea / ByteRealms (@FranGallegoBR)
                              4 ;;
                              5 ;;  This program is free software: you can redistribute it and/or modify
                              6 ;;  it under the terms of the GNU Lesser General Public License as published by
                              7 ;;  the Free Software Foundation, either version 3 of the License, or
                              8 ;;  (at your option) any later version.
                              9 ;;
                             10 ;;  This program is distributed in the hope that it will be useful,
                             11 ;;  but WITHOUT ANY WARRANTY; without even the implied warranty of
                             12 ;;  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
                             13 ;;  GNU Lesser General Public License for more details.
                             14 ;;
                             15 ;;  You should have received a copy of the GNU Lesser General Public License
                             16 ;;  along with this program.  If not, see <http://www.gnu.org/licenses/>.
                             17 ;;-------------------------------------------------------------------------------
                             18 
                             19 ;;//////////////////////////////////////////////////////////////////////
                             20 ;;//////////////////////////////////////////////////////////////////////
                             21 ;; File: Colours (asm)
                             22 ;;//////////////////////////////////////////////////////////////////////
                             23 ;;//////////////////////////////////////////////////////////////////////
                             24 ;;
                             25 ;;    Constants and utilities to manage the 27 colours from
                             26 ;; the CPC Palette comfortably in assembler.
                             27 ;;
                             28 ;;
                             29 
                             30 ;; Constant: Firmware colour values
                             31 ;;
                             32 ;;    Enumerates all 27 firmware colours for assembler programs
                             33 ;;
                             34 ;; Values:
                             35 ;; (start code)
                             36 ;;   [=================================================]
                             37 ;;   | Identifier        | Val| Identifier        | Val|
                             38 ;;   |-------------------------------------------------|
                             39 ;;   | FW_BLACK          |  0 | FW_BLUE           |  1 |
                             40 ;;   | FW_BRIGHT_BLUE    |  2 | FW_RED            |  3 |
                             41 ;;   | FW_MAGENTA        |  4 | FW_MAUVE          |  5 |
                             42 ;;   | FW_BRIGHT_RED     |  6 | FW_PURPLE         |  7 |
                             43 ;;   | FW_BRIGHT_MAGENTA |  8 | FW_GREEN          |  9 |
                             44 ;;   | FW_CYAN           | 10 | FW_SKY_BLUE       | 11 |
                             45 ;;   | FW_YELLOW         | 12 | FW_WHITE          | 13 |
                             46 ;;   | FW_PASTEL_BLUE    | 14 | FW_ORANGE         | 15 |
                             47 ;;   | FW_PINK           | 16 | FW_PASTEL_MAGENTA | 17 |
                             48 ;;   | FW_BRIGHT_GREEN   | 18 | FW_SEA_GREEN      | 19 |
                             49 ;;   | FW_BRIGHT_CYAN    | 20 | FW_LIME           | 21 |
                             50 ;;   | FW_PASTEL_GREEN   | 22 | FW_PASTEL_CYAN    | 23 |
                             51 ;;   | FW_BRIGHT_YELLOW  | 24 | FW_PASTEL_YELLOW  | 25 |
                             52 ;;   | FW_BRIGHT_WHITE   | 26 |                   |    |
                             53 ;;   [=================================================]
                             54 ;; (end code)
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 11.
Hexadecimal [16-Bits]



                             55 
                     0000    56 FW_BLACK          =  0
                     0001    57 FW_BLUE           =  1
                     0002    58 FW_BRIGHT_BLUE    =  2
                     0003    59 FW_RED            =  3
                     0004    60 FW_MAGENTA        =  4
                     0005    61 FW_MAUVE          =  5
                     0006    62 FW_BRIGHT_RED     =  6
                     0007    63 FW_PURPLE         =  7
                     0008    64 FW_BRIGHT_MAGENTA =  8
                     0009    65 FW_GREEN          =  9
                     000A    66 FW_CYAN           = 10
                     000B    67 FW_SKY_BLUE       = 11
                     000C    68 FW_YELLOW         = 12
                     000D    69 FW_WHITE          = 13
                     000E    70 FW_PASTEL_BLUE    = 14
                     000F    71 FW_ORANGE         = 15
                     0010    72 FW_PINK           = 16
                     0011    73 FW_PASTEL_MAGENTA = 17
                     0012    74 FW_BRIGHT_GREEN   = 18
                     0013    75 FW_SEA_GREEN      = 19
                     0014    76 FW_BRIGHT_CYAN    = 20
                     0015    77 FW_LIME           = 21
                     0016    78 FW_PASTEL_GREEN   = 22
                     0017    79 FW_PASTEL_CYAN    = 23
                     0018    80 FW_BRIGHT_YELLOW  = 24
                     0019    81 FW_PASTEL_YELLOW  = 25
                     001A    82 FW_BRIGHT_WHITE   = 26
                             83 
                             84 ;; Constant: Hardware colour values
                             85 ;;
                             86 ;;    Enumerates all 27 hardware colours for assembler programs
                             87 ;;
                             88 ;; Values:
                             89 ;; (start code)
                             90 ;;   [=====================================================]
                             91 ;;   | Identifier        | Value| Identifier        | Value|
                             92 ;;   |-----------------------------------------------------|
                             93 ;;   | HW_BLACK          | 0x14 | HW_BLUE           | 0x04 |
                             94 ;;   | HW_BRIGHT_BLUE    | 0x15 | HW_RED            | 0x1C |
                             95 ;;   | HW_MAGENTA        | 0x18 | HW_MAUVE          | 0x1D |
                             96 ;;   | HW_BRIGHT_RED     | 0x0C | HW_PURPLE         | 0x05 |
                             97 ;;   | HW_BRIGHT_MAGENTA | 0x0D | HW_GREEN          | 0x16 |
                             98 ;;   | HW_CYAN           | 0x06 | HW_SKY_BLUE       | 0x17 |
                             99 ;;   | HW_YELLOW         | 0x1E | HW_WHITE          | 0x00 |
                            100 ;;   | HW_PASTEL_BLUE    | 0x1F | HW_ORANGE         | 0x0E |
                            101 ;;   | HW_PINK           | 0x07 | HW_PASTEL_MAGENTA | 0x0F |
                            102 ;;   | HW_BRIGHT_GREEN   | 0x12 | HW_SEA_GREEN      | 0x02 |
                            103 ;;   | HW_BRIGHT_CYAN    | 0x13 | HW_LIME           | 0x1A |
                            104 ;;   | HW_PASTEL_GREEN   | 0x19 | HW_PASTEL_CYAN    | 0x1B |
                            105 ;;   | HW_BRIGHT_YELLOW  | 0x0A | HW_PASTEL_YELLOW  | 0x03 |
                            106 ;;   | HW_BRIGHT_WHITE   | 0x0B |                   |      |
                            107 ;;   [=====================================================]
                            108 ;; (end code)
                            109 ;;
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 12.
Hexadecimal [16-Bits]



                     0014   110 HW_BLACK          = 0x14
                     0004   111 HW_BLUE           = 0x04
                     0015   112 HW_BRIGHT_BLUE    = 0x15
                     001C   113 HW_RED            = 0x1C
                     0018   114 HW_MAGENTA        = 0x18
                     001D   115 HW_MAUVE          = 0x1D
                     000C   116 HW_BRIGHT_RED     = 0x0C
                     0005   117 HW_PURPLE         = 0x05
                     000D   118 HW_BRIGHT_MAGENTA = 0x0D
                     0016   119 HW_GREEN          = 0x16
                     0006   120 HW_CYAN           = 0x06
                     0017   121 HW_SKY_BLUE       = 0x17
                     001E   122 HW_YELLOW         = 0x1E
                     0000   123 HW_WHITE          = 0x00
                     001F   124 HW_PASTEL_BLUE    = 0x1F
                     000E   125 HW_ORANGE         = 0x0E
                     0007   126 HW_PINK           = 0x07
                     000F   127 HW_PASTEL_MAGENTA = 0x0F
                     0012   128 HW_BRIGHT_GREEN   = 0x12
                     0002   129 HW_SEA_GREEN      = 0x02
                     0013   130 HW_BRIGHT_CYAN    = 0x13
                     001A   131 HW_LIME           = 0x1A
                     0019   132 HW_PASTEL_GREEN   = 0x19
                     001B   133 HW_PASTEL_CYAN    = 0x1B
                     000A   134 HW_BRIGHT_YELLOW  = 0x0A
                     0003   135 HW_PASTEL_YELLOW  = 0x03
                     000B   136 HW_BRIGHT_WHITE   = 0x0B
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 13.
Hexadecimal [16-Bits]



                              5 
                              6 .area _CODE
                              7 
   4B16 00 00                 8 segundos: .dw 0x00
                              9 
                             10 
                             11 ;; ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
                             12 ;; ;;;;;;; EN IX DIRECCIÓN ENEMIGO ;;;;;;;;;;;;;;;
                             13 ;; ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
   4B18                      14 situaEnemigos::
   4B18 3A 85 4E      [13]   15     ld  a, (num_enemigos)
   4B1B                      16 next:
   4B1B CD 27 4B      [17]   17     call pideX
   4B1E 3D            [ 4]   18     dec a
   4B1F C8            [11]   19     ret z
   4B20 01 14 00      [10]   20     ld  bc, #tamagno_enemy
   4B23 DD 09         [15]   21     add ix, bc
   4B25 18 F4         [12]   22     jr next
                             23 
   4B27                      24 pideX:
   4B27 F5            [11]   25     push af                                                ;; Preservar A por que calXenemy lo modifica
   4B28 CD 88 4E      [17]   26     call calXenemy
   4B2B F1            [10]   27     pop  af                                                ;; Se recupera el antiguo valor de A
   4B2C C9            [10]   28     ret
                             29 
   4B2D                      30 reloj::
   4B2D 2A 16 4B      [16]   31     ld  hl, (segundos)                                     ;; Número de segundos transcurridos
   4B30 7C            [ 4]   32     ld  a, h
   4B31 FE 0B         [ 7]   33     cp #0x0B
   4B33 20 0E         [12]   34     jr nz, suma
   4B35 7D            [ 4]   35     ld  a, l
   4B36 FE B8         [ 7]   36     cp #0xB8
   4B38 20 09         [12]   37     jr nz, suma
   0024                      38     cpctm_setBorder_asm HW_WHITE
                              1    .radix h
   0024                       2    cpctm_setBorder_raw_asm \HW_WHITE ;; [28] Macro that does the job, but requires a number value to be passed
                              1    .globl cpct_setPALColour_asm
   4B3A 21 10 00      [10]    2    ld   hl, #0x010         ;; [3]  H=Hardware value of desired colour, L=Border INK (16)
   4B3D CD 7B 55      [17]    3    call cpct_setPALColour_asm  ;; [25] Set Palette colour of the border
                              3    .radix d
   4B40 36 00         [10]   39     ld (hl), #0x0000
   4B42 C9            [10]   40     ret
   4B43                      41 suma:
   4B43 23            [ 6]   42     inc hl
   4B44 22 16 4B      [16]   43     ld (segundos), hl
   4B47 C9            [10]   44     ret
