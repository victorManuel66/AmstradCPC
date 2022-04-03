ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 1.
Hexadecimal [16-Bits]



                              1 .area _CODE
                              2 
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 2.
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 3.
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 4.
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 5.
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 6.
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 7.
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 8.
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 9.
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 10.
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 11.
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 12.
Hexadecimal [16-Bits]



                              5 .include "cpctelera.h.s"
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 13.
Hexadecimal [16-Bits]



                              6 .include "mensajes.h.s"
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 14.
Hexadecimal [16-Bits]



                              7 .include "keyboard/keyboard.h.s"
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
                             18 .module cpct_keyboard
                             19 
                             20 ;;
                             21 ;; Constant: Key Definitions (asm)
                             22 ;;
                             23 ;;    Definitions of the KeyCodes required by <cpct_isKeyPressed> 
                             24 ;; function for assembler programs. These are 16-bit values that define 
                             25 ;; matrix line in the keyboard layout (Most Significant Byte) and bit to
                             26 ;; be tested in that matrix line status for the given key (Least Significant
                             27 ;; byte). Each matrix line in the keyboard returns a byte containing the
                             28 ;; status of 8 keys, 1 bit each.
                             29 ;;
                             30 ;; CPCtelera include file:
                             31 ;;    _keyboard/keyboard.h.s_
                             32 ;;
                             33 ;; Keycode constant names:
                             34 ;; (start code)
                             35 ;;  KeyCode | Constant        || KeyCode | Constant      || KeyCode |  Constant
                             36 ;; -------------------------------------------------------------------------------
                             37 ;;   0x0100 | Key_CursorUp    ||  0x0803 | Key_P         ||  0x4006 |  Key_B
                             38 ;;          |                 ||         |               ||     ''  |  Joy1_Fire3
                             39 ;;   0x0200 | Key_CursorRight ||  0x1003 | Key_SemiColon ||  0x8006 |  Key_V
                             40 ;;   0x0400 | Key_CursorDown  ||  0x2003 | Key_Colon     ||  0x0107 |  Key_4
                             41 ;;   0x0800 | Key_F9          ||  0x4003 | Key_Slash     ||  0x0207 |  Key_3
                             42 ;;   0x1000 | Key_F6          ||  0x8003 | Key_Dot       ||  0x0407 |  Key_E
                             43 ;;   0x2000 | Key_F3          ||  0x0104 | Key_0         ||  0x0807 |  Key_W
                             44 ;;   0x4000 | Key_Enter       ||  0x0204 | Key_9         ||  0x1007 |  Key_S
                             45 ;;   0x8000 | Key_FDot        ||  0x0404 | Key_O         ||  0x2007 |  Key_D
                             46 ;;   0x0101 | Key_CursorLeft  ||  0x0804 | Key_I         ||  0x4007 |  Key_C
                             47 ;;   0x0201 | Key_Copy        ||  0x1004 | Key_L         ||  0x8007 |  Key_X
                             48 ;;   0x0401 | Key_F7          ||  0x2004 | Key_K         ||  0x0108 |  Key_1
                             49 ;;   0x0801 | Key_F8          ||  0x4004 | Key_M         ||  0x0208 |  Key_2
                             50 ;;   0x1001 | Key_F5          ||  0x8004 | Key_Comma     ||  0x0408 |  Key_Esc
                             51 ;;   0x2001 | Key_F1          ||  0x0105 | Key_8         ||  0x0808 |  Key_Q
                             52 ;;   0x4001 | Key_F2          ||  0x0205 | Key_7         ||  0x1008 |  Key_Tab
                             53 ;;   0x8001 | Key_F0          ||  0x0405 | Key_U         ||  0x2008 |  Key_A
                             54 ;;   0x0102 | Key_Clr         ||  0x0805 | Key_Y         ||  0x4008 |  Key_CapsLock
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 15.
Hexadecimal [16-Bits]



                             55 ;;   0x0202 | Key_OpenBracket ||  0x1005 | Key_H         ||  0x8008 |  Key_Z
                             56 ;;   0x0402 | Key_Return      ||  0x2005 | Key_J         ||  0x0109 |  Joy0_Up
                             57 ;;   0x0802 | Key_CloseBracket||  0x4005 | Key_N         ||  0x0209 |  Joy0_Down
                             58 ;;   0x1002 | Key_F4          ||  0x8005 | Key_Space     ||  0x0409 |  Joy0_Left
                             59 ;;   0x2002 | Key_Shift       ||  0x0106 | Key_6         ||  0x0809 |  Joy0_Right
                             60 ;;          |                 ||     ''  | Joy1_Up       ||         |
                             61 ;;   0x4002 | Key_BackSlash   ||  0x0206 | Key_5         ||  0x1009 |  Joy0_Fire1
                             62 ;;          |                 ||     ''  | Joy1_Down     ||         |
                             63 ;;   0x8002 | Key_Control     ||  0x0406 | Key_R         ||  0x2009 |  Joy0_Fire2
                             64 ;;          |                 ||     ''  | Joy1_Left     ||         |
                             65 ;;   0x0103 | Key_Caret       ||  0x0806 | Key_T         ||  0x4009 |  Joy0_Fire3
                             66 ;;          |                 ||     ''  | Joy1 Right    ||
                             67 ;;   0x0203 | Key_Hyphen      ||  0x1006 | Key_G         ||  0x8009 |  Key_Del
                             68 ;;          |                 ||     ''  | Joy1_Fire1    ||
                             69 ;;   0x0403 | Key_At          ||  0x2006 | Key_F         ||
                             70 ;;          |                 ||     ''  | Joy1_Fire2    ||
                             71 ;; -------------------------------------------------------------------------------
                             72 ;;  Table 1. KeyCodes defined for each possible key, ordered by KeyCode
                             73 ;; (end)
                             74 ;;
                             75 
                             76 ;; Matrix Line 0x00
                     0100    77 Key_CursorUp     = #0x0100  ;; Bit 0 (01h) => | 0000 0001 |
                     0200    78 Key_CursorRight  = #0x0200  ;; Bit 1 (02h) => | 0000 0010 |
                     0400    79 Key_CursorDown   = #0x0400  ;; Bit 2 (04h) => | 0000 0100 |
                     0800    80 Key_F9           = #0x0800  ;; Bit 3 (08h) => | 0000 1000 |
                     1000    81 Key_F6           = #0x1000  ;; Bit 4 (10h) => | 0001 0000 |
                     2000    82 Key_F3           = #0x2000  ;; Bit 5 (20h) => | 0010 0000 |
                     4000    83 Key_Enter        = #0x4000  ;; Bit 6 (40h) => | 0100 0000 |
                     8000    84 Key_FDot         = #0x8000  ;; Bit 7 (80h) => | 1000 0000 |
                             85 ;; Matrix Line 0x01
                     0101    86 Key_CursorLeft   = #0x0101
                     0201    87 Key_Copy         = #0x0201
                     0401    88 Key_F7           = #0x0401
                     0801    89 Key_F8           = #0x0801
                     1001    90 Key_F5           = #0x1001
                     2001    91 Key_F1           = #0x2001
                     4001    92 Key_F2           = #0x4001
                     8001    93 Key_F0           = #0x8001
                             94 ;; Matrix Line 0x02
                     0102    95 Key_Clr          = #0x0102
                     0202    96 Key_OpenBracket  = #0x0202
                     0402    97 Key_Return       = #0x0402
                     0802    98 Key_CloseBracket = #0x0802
                     1002    99 Key_F4           = #0x1002
                     2002   100 Key_Shift        = #0x2002
                     4002   101 Key_BackSlash    = #0x4002
                     8002   102 Key_Control      = #0x8002
                            103 ;; Matrix Line 0x03
                     0103   104 Key_Caret        = #0x0103
                     0203   105 Key_Hyphen       = #0x0203
                     0403   106 Key_At           = #0x0403
                     0803   107 Key_P            = #0x0803
                     1003   108 Key_SemiColon    = #0x1003
                     2003   109 Key_Colon        = #0x2003
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 16.
Hexadecimal [16-Bits]



                     4003   110 Key_Slash        = #0x4003
                     8003   111 Key_Dot          = #0x8003
                            112 ;; Matrix Line 0x04
                     0104   113 Key_0            = #0x0104
                     0204   114 Key_9            = #0x0204
                     0404   115 Key_O            = #0x0404
                     0804   116 Key_I            = #0x0804
                     1004   117 Key_L            = #0x1004
                     2004   118 Key_K            = #0x2004
                     4004   119 Key_M            = #0x4004
                     8004   120 Key_Comma        = #0x8004
                            121 ;; Matrix Line 0x05
                     0105   122 Key_8            = #0x0105
                     0205   123 Key_7            = #0x0205
                     0405   124 Key_U            = #0x0405
                     0805   125 Key_Y            = #0x0805
                     1005   126 Key_H            = #0x1005
                     2005   127 Key_J            = #0x2005
                     4005   128 Key_N            = #0x4005
                     8005   129 Key_Space        = #0x8005
                            130 ;; Matrix Line 0x06
                     0106   131 Key_6            = #0x0106
                     0106   132 Joy1_Up          = #0x0106
                     0206   133 Key_5            = #0x0206
                     0206   134 Joy1_Down        = #0x0206
                     0406   135 Key_R            = #0x0406
                     0406   136 Joy1_Left        = #0x0406
                     0806   137 Key_T            = #0x0806
                     0806   138 Joy1_Right       = #0x0806
                     1006   139 Key_G            = #0x1006
                     1006   140 Joy1_Fire1       = #0x1006
                     2006   141 Key_F            = #0x2006
                     2006   142 Joy1_Fire2       = #0x2006
                     4006   143 Key_B            = #0x4006
                     4006   144 Joy1_Fire3       = #0x4006
                     8006   145 Key_V            = #0x8006
                            146 ;; Matrix Line 0x07
                     0107   147 Key_4            = #0x0107
                     0207   148 Key_3            = #0x0207
                     0407   149 Key_E            = #0x0407
                     0807   150 Key_W            = #0x0807
                     1007   151 Key_S            = #0x1007
                     2007   152 Key_D            = #0x2007
                     4007   153 Key_C            = #0x4007
                     8007   154 Key_X            = #0x8007
                            155 ;; Matrix Line 0x08
                     0108   156 Key_1            = #0x0108
                     0208   157 Key_2            = #0x0208
                     0408   158 Key_Esc          = #0x0408
                     0808   159 Key_Q            = #0x0808
                     1008   160 Key_Tab          = #0x1008
                     2008   161 Key_A            = #0x2008
                     4008   162 Key_CapsLock     = #0x4008
                     8008   163 Key_Z            = #0x8008
                            164 ;; Matrix Line 0x09
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 17.
Hexadecimal [16-Bits]



                     0109   165 Joy0_Up          = #0x0109
                     0209   166 Joy0_Down        = #0x0209
                     0409   167 Joy0_Left        = #0x0409
                     0809   168 Joy0_Right       = #0x0809
                     1009   169 Joy0_Fire1       = #0x1009
                     2009   170 Joy0_Fire2       = #0x2009
                     4009   171 Joy0_Fire3       = #0x4009
                     8009   172 Key_Del          = #0x8009
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 18.
Hexadecimal [16-Bits]



                              8 .include "Player.h.s"
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
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 19.
Hexadecimal [16-Bits]



                              9 
                             10 .globl _cpct_keyboardStatusBuffer           ;; Array que contiene el estado del teclado
                             11 
   505F                      12 menu::
   0000                      13     cpctm_clearScreen_asm 0x00              ;; Borra la pantalla
   505F 21 00 C0      [10]    1    ld    hl, #0xC000    ;; [3] HL Points to Start of Video Memory
   5062 11 01 C0      [10]    2    ld    de, #0xC001    ;; [3] DE Points to the next byte
   5065 01 FF 3F      [10]    3    ld    bc, #(0x4000-1);; [3] BC = 16383 bytes to be copied
   5068 36 00         [10]    4    ld   (hl), #0x00      ;; [3] First Byte = given Colour
   506A ED B0         [21]    5    ldir                 ;; [98297] Perform the copy
   000D                      14     cpctm_setBorder_asm HW_BLACK            ;; Borde de la pantalla a negro
                              1    .radix h
   000D                       2    cpctm_setBorder_raw_asm \HW_BLACK ;; [28] Macro that does the job, but requires a number value to be passed
                              1    .globl cpct_setPALColour_asm
   506C 21 10 14      [10]    2    ld   hl, #0x1410         ;; [3]  H=Hardware value of desired colour, L=Border INK (16)
   506F CD 8B 55      [17]    3    call cpct_setPALColour_asm  ;; [25] Set Palette colour of the border
                              3    .radix d
                             15 
   5072 21 06 00      [10]   16     ld hl, #0x0006
   5075 CD D6 60      [17]   17     call cpct_setDrawCharM0_asm             ;; Establecer color del fondo y de la pluma
                             18 
   5078 11 00 C0      [10]   19     ld de, #0xC000
   507B 01 0F 4C      [10]   20     ld bc, #0x4C0F                          ;; Coordenada Y en B, coordenada X en C
   507E CD F9 60      [17]   21     call cpct_getScreenPtr_asm
                             22 
   5081 FD 21 0D 4C   [14]   23     ld iy, #writeMenu1                      ;; Dirección del texto
   5085 CD BE 5D      [17]   24     call cpct_drawStringM0_asm              ;; Escribe
                             25 
   5088 11 00 C0      [10]   26     ld de, #0xC000
   508B 01 0F 55      [10]   27     ld bc, #0x550F                          ;; Coordenada Y en B, coordenada X en C
   508E CD F9 60      [17]   28     call cpct_getScreenPtr_asm
                             29 
   5091 FD 21 16 4C   [14]   30     ld iy, #writeMenu2                      ;; Dirección del texto
   5095 CD BE 5D      [17]   31     call cpct_drawStringM0_asm              ;; Escribe
                             32 
   5098 11 00 C0      [10]   33     ld de, #0xC000
   509B 01 0F 5E      [10]   34     ld bc, #0x5E0F                          ;; Coordenada Y en B, coordenada X en C
   509E CD F9 60      [17]   35     call cpct_getScreenPtr_asm
                             36 
   50A1 FD 21 23 4C   [14]   37     ld iy, #writeMenu3                      ;; Dirección del texto
   50A5 CD BE 5D      [17]   38     call cpct_drawStringM0_asm              ;; Escribe
                             39 
   50A8                      40 cont:
   50A8 CD 0D 61      [17]   41     call cpct_scanKeyboard_asm              ;; Escanea el teclado
                             42 
   50AB 21 08 01      [10]   43     ld hl, #Key_1
   50AE CD 7F 55      [17]   44     call cpct_isKeyPressed_asm
   50B1 20 12         [12]   45     jr  nz, Pulsado1
   50B3 21 08 02      [10]   46     ld hl, #Key_2
   50B6 CD 7F 55      [17]   47     call cpct_isKeyPressed_asm
   50B9 20 0D         [12]   48     jr  nz, Pulsado2
   50BB 21 07 02      [10]   49     ld hl, #Key_3
   50BE CD 7F 55      [17]   50     call cpct_isKeyPressed_asm
   50C1 20 0A         [12]   51     jr  nz, Pulsado3
   50C3 18 E3         [12]   52     jr  cont
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 20.
Hexadecimal [16-Bits]



   50C5                      53 Pulsado1:
   50C5 3E 01         [ 7]   54     ld  a, #0x01
   50C7 C9            [10]   55     ret
   50C8                      56 Pulsado2:
   50C8 CD 0D 61      [17]   57     call cpct_scanKeyboard_asm                 ;; Para resetear el teclado
   50CB 18 13         [12]   58     jr redefine
   50CD                      59 Pulsado3:
                             60     ;; Asignar izquierda
   50CD 21 09 04      [10]   61     ld hl, #Joy0_Left
   50D0 22 91 49      [16]   62     ld (TeclaIz), hl
   50D3 21 09 08      [10]   63     ld hl, #Joy0_Right
   50D6 22 8F 49      [16]   64     ld (TeclaDe), hl
   50D9 21 09 10      [10]   65     ld hl, #Joy0_Fire1
   50DC 22 93 49      [16]   66     ld (TeclaDi), hl
   50DF C9            [10]   67     ret
                             68 
   50E0                      69 redefine:
   0081                      70     cpctm_clearScreen_asm 0x00              ;; Borra la pantalla
   50E0 21 00 C0      [10]    1    ld    hl, #0xC000    ;; [3] HL Points to Start of Video Memory
   50E3 11 01 C0      [10]    2    ld    de, #0xC001    ;; [3] DE Points to the next byte
   50E6 01 FF 3F      [10]    3    ld    bc, #(0x4000-1);; [3] BC = 16383 bytes to be copied
   50E9 36 00         [10]    4    ld   (hl), #0x00      ;; [3] First Byte = given Colour
   50EB ED B0         [21]    5    ldir                 ;; [98297] Perform the copy
                             71 
   50ED 11 00 C0      [10]   72     ld de, #0xC000
   50F0 01 0A 4C      [10]   73     ld bc, #0x4C0A
   50F3 CD F9 60      [17]   74     call cpct_getScreenPtr_asm
                             75 
   50F6 FD 21 30 4C   [14]   76     ld iy, #izqui                     ;; Dirección del texto
   50FA CD BE 5D      [17]   77     call cpct_drawStringM0_asm        ;; Escribe
   50FD                      78 keyleft:
   50FD CD 89 51      [17]   79     call delay
   5100 CD 0D 61      [17]   80     call cpct_scanKeyboard_asm        ;; Escaneo el teclado
   5103 CD BA 5F      [17]   81     call cpct_isAnyKeyPressed_asm
   5106 28 F5         [12]   82     jr z, keyleft
   5108 CD 8F 51      [17]   83     call drawKey                      ;; Localiza la tecla que ha pulsado el usuario
   510B 22 91 49      [16]   84     ld (TeclaIz), hl                  ;; HL contiene el cpct_keyID
   510E D5            [11]   85     push  de                          ;; Preservar el valor devuelto por drawKey
   510F 11 00 C0      [10]   86     ld de, #0xC000
   5112 01 26 4C      [10]   87     ld bc, #0x4C26
   5115 CD F9 60      [17]   88     call cpct_getScreenPtr_asm        ;; Hace un locate 
   5118 01 06 00      [10]   89     ld  bc, #0x0006
   511B D1            [10]   90     pop  de                           ;; Recuperar de la pila el caracter devuelto por drawKeyLeft
   511C CD 78 5F      [17]   91     call cpct_drawCharM0_asm          ;; Imprime la tecla
                             92 
                             93     
   511F 11 00 C0      [10]   94     ld de, #0xC000
   5122 01 0A 55      [10]   95     ld bc, #0x550A
   5125 CD F9 60      [17]   96     call cpct_getScreenPtr_asm
   5128 FD 21 3A 4C   [14]   97     ld iy, #dere                      ;; Dirección del texto
   512C CD BE 5D      [17]   98     call cpct_drawStringM0_asm        ;; Escribe
   512F CD 89 51      [17]   99     call delay                        ;; Para que a scanKeyBoard no sea tan rápido
                            100 
   5132                     101 keyright:
   5132 CD 0D 61      [17]  102     call cpct_scanKeyboard_asm        ;; Escaneo el teclado
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 21.
Hexadecimal [16-Bits]



   5135 CD BA 5F      [17]  103     call cpct_isAnyKeyPressed_asm
   5138 28 F8         [12]  104     jr  z, keyright
   513A CD 8F 51      [17]  105     call drawKey                      ;; Localiza la tecla que ha pulsado el usuario
   513D 22 8F 49      [16]  106     ld (TeclaDe), hl                  ;; HL contiene el cpct_keyID
   5140 D5            [11]  107     push de                           ;; Preservar el valor devuelto por drawKey
   5141 11 00 C0      [10]  108     ld de, #0xC000
   5144 01 26 54      [10]  109     ld bc, #0x5426
   5147 CD F9 60      [17]  110     call cpct_getScreenPtr_asm        ;; Hace un locate 
   514A 01 06 00      [10]  111     ld  bc, #0x0006
   514D D1            [10]  112     pop  de                           ;; Recuperar de la pila el caracter devuelto por drawKeyLeft
   514E CD 78 5F      [17]  113     call cpct_drawCharM0_asm          ;; Imprime la tecla
   5151 CD 89 51      [17]  114     call delay                        ;; Para que a scanKeyBoard no sea tan rápido
                            115 
   5154 11 00 C0      [10]  116     ld de, #0xC000
   5157 01 0A 5E      [10]  117     ld bc, #0x5E0A
   515A CD F9 60      [17]  118     call cpct_getScreenPtr_asm
   515D FD 21 43 4C   [14]  119     ld iy, #fue                       ;; Dirección del texto
   5161 CD BE 5D      [17]  120     call cpct_drawStringM0_asm        ;; Escribe
   5164                     121 keyfire:
                            122     ;call delay                        ;; Para que a scanKeyBoard no sea tan rápido
   5164 CD 0D 61      [17]  123     call cpct_scanKeyboard_asm        ;; Escaneo el teclado
   5167 CD BA 5F      [17]  124     call cpct_isAnyKeyPressed_asm
   516A 28 F8         [12]  125     jr  z, keyfire
   516C CD 8F 51      [17]  126     call drawKey                      ;; Localiza la tecla que ha pulsado el usuario
   516F 22 93 49      [16]  127     ld (TeclaDi), hl                  ;; HL contiene el cpct_keyID
   5172 D5            [11]  128     push de                           ;; Preservar el valor devuelto por drawKey
   5173 11 00 C0      [10]  129     ld de, #0xC000
   5176 01 26 5C      [10]  130     ld bc, #0x5C26
   5179 CD F9 60      [17]  131     call cpct_getScreenPtr_asm        ;; Hace un locate 
   517C 01 06 00      [10]  132     ld  bc, #0x0006
   517F D1            [10]  133     pop  de                           ;; Recuperar de la pila el caracter devuelto por drawKeyLeft
   5180 CD 78 5F      [17]  134     call cpct_drawCharM0_asm          ;; Imprime la tecla
                            135 
   5183 CD 89 51      [17]  136     call delay                        ;; Para que a scanKeyBoard no sea tan rápido
                            137 
   5186 C3 5F 50      [10]  138     jp menu
                            139 
   5189                     140 delay:
   5189 06 96         [ 7]  141     ld  b, #0x96
   518B                     142 espera:
   518B 76            [ 4]  143     halt
   518C 10 FD         [13]  144     djnz espera
   518E C9            [10]  145     ret
                            146 
   518F                     147 drawKey:
   518F 21 8F 5F      [10]  148     ld hl, #_cpct_keyboardStatusBuffer
                            149 
   5192 06 00         [ 7]  150     ld  b, #0x00                     ;; Contador de los bytes del array
   5194                     151 otraLinea:                           ;; Primero hay que encontrar la línea
   5194 7E            [ 7]  152     ld   a, (hl)                     ;; El byte al acumulador
   5195 23            [ 6]  153     inc hl                           ;; Apunta al siguiente elemento de la tabla
   5196 04            [ 4]  154     inc  b                           ;; Para que B conozca el indice del array
   5197 FE FF         [ 7]  155     cp  #0xFF                        ;; Ver si se pulso la tecla de esa línea
   5199 28 F9         [12]  156     jr  z, otraLinea
   519B CD 9F 51      [17]  157     call whoKey                      ;; Ahora hay que buscar la tecla
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 22.
Hexadecimal [16-Bits]



                            158 
   519E C9            [10]  159     ret
                            160 
   519F                     161 whoKey:
   519F F5            [11]  162     push af                          ;; Guardar tecla pulsada
   51A0 78            [ 4]  163     ld  a, b                         ;; Llevamos la fila al acumulador
   51A1 FE 0A         [ 7]  164     cp  #0x0A                        ;; Última fila
   51A3 CA D3 51      [10]  165     jp  z, _49
   51A6 FE 09         [ 7]  166     cp  #0x09
   51A8 CA 2B 52      [10]  167     jp  z, _48
   51AB FE 08         [ 7]  168     cp  #0x08
   51AD CA 83 52      [10]  169     jp  z, _47
   51B0 FE 07         [ 7]  170     cp  #0x07
   51B2 CA DB 52      [10]  171     jp  z, _46
   51B5 FE 06         [ 7]  172     cp  #0x06
   51B7 CA 34 53      [10]  173     jp  z, _45
   51BA FE 05         [ 7]  174     cp  #0x05
   51BC CA 8C 53      [10]  175     jp  z, _44
   51BF FE 04         [ 7]  176     cp  #0x04
   51C1 CA E4 53      [10]  177     jp  z, _43
   51C4 FE 03         [ 7]  178     cp  #0x03
   51C6 CA 3C 54      [10]  179     jp  z, _42
   51C9 FE 02         [ 7]  180     cp  #0x02
   51CB CA 94 54      [10]  181     jp  z, _41
   51CE FE 01         [ 7]  182     cp  #0x01
   51D0 CA EC 54      [10]  183     jp  z, _40
   51D3                     184 _49:
   51D3 F1            [10]  185     pop af                          ;; Recupero tecla pulsada
   51D4 06 7F         [ 7]  186     ld  b, #0x7F                    
   51D6 B8            [ 4]  187     cp  b
   51D7 20 06         [12]  188     jr  nz, bit6
   51D9 1E FE         [ 7]  189     ld  e, #254
   51DB 21 09 80      [10]  190     ld  hl, #Key_Del
   51DE C9            [10]  191     ret
   51DF                     192 bit6:
   51DF CB 08         [ 8]  193     rrc b
   51E1 B8            [ 4]  194     cp  b
   51E2 20 06         [12]  195     jr nz, bit5
   51E4 1E FF         [ 7]  196     ld  e, #255
   51E6 21 09 40      [10]  197     ld  hl, #Joy0_Fire3
   51E9 C9            [10]  198     ret
   51EA                     199 bit5:
   51EA CB 08         [ 8]  200     rrc b
   51EC B8            [ 4]  201     cp  b
   51ED 20 06         [12]  202     jr nz, bit4
   51EF 1E FD         [ 7]  203     ld  e, #253
   51F1 21 09 20      [10]  204     ld  hl, #Joy0_Fire2
   51F4 C9            [10]  205     ret
   51F5                     206 bit4:
   51F5 CB 08         [ 8]  207     rrc b
   51F7 B8            [ 4]  208     cp  b
   51F8 20 06         [12]  209     jr nz, bit3
   51FA 1E FC         [ 7]  210     ld  e, #252
   51FC 21 09 10      [10]  211     ld  hl, #Joy0_Fire1
   51FF C9            [10]  212     ret
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 23.
Hexadecimal [16-Bits]



   5200                     213 bit3:
   5200 CB 08         [ 8]  214     rrc b
   5202 B8            [ 4]  215     cp  b
   5203 20 06         [12]  216     jr nz, bit2
   5205 1E F3         [ 7]  217     ld  e, #243
   5207 21 09 08      [10]  218     ld  hl, #Joy0_Right
   520A C9            [10]  219     ret
   520B                     220 bit2:
   520B CB 08         [ 8]  221     rrc b
   520D B8            [ 4]  222     cp  b
   520E 20 06         [12]  223     jr nz, bit1
   5210 1E F2         [ 7]  224     ld  e, #242
   5212 21 09 04      [10]  225     ld  hl, #Joy0_Left
   5215 C9            [10]  226     ret
   5216                     227 bit1:
   5216 CB 08         [ 8]  228     rrc b
   5218 B8            [ 4]  229     cp  b
   5219 20 06         [12]  230     jr nz, bit0
   521B 1E F1         [ 7]  231     ld  e, #241
   521D 21 09 02      [10]  232     ld  hl, #Joy0_Down
   5220 C9            [10]  233     ret
   5221                     234 bit0:
   5221 CB 08         [ 8]  235     rrc b
   5223 B8            [ 4]  236     cp  b
   5224 C0            [11]  237     ret nz
   5225 1E F0         [ 7]  238     ld  e, #240
   5227 21 09 01      [10]  239     ld  hl, #Joy0_Up
   522A C9            [10]  240     ret
                            241 
   522B                     242 _48:
   522B F1            [10]  243     pop af                          ;; Recupero tecla pulsada
   522C 06 7F         [ 7]  244     ld  b, #0x7F                    
   522E B8            [ 4]  245     cp  b
   522F 20 06         [12]  246     jr  nz, _48bit6
   5231 1E 5A         [ 7]  247     ld  e, #90
   5233 21 08 80      [10]  248     ld  hl, #Key_Z
   5236 C9            [10]  249     ret
   5237                     250 _48bit6:
   5237 CB 08         [ 8]  251     rrc b
   5239 B8            [ 4]  252     cp  b
   523A 20 06         [12]  253     jr nz, _48bit5
   523C 1E E9         [ 7]  254     ld  e, #233
   523E 21 08 40      [10]  255     ld  hl, #Key_CapsLock
   5241 C9            [10]  256     ret
   5242                     257 _48bit5:
   5242 CB 08         [ 8]  258     rrc b
   5244 B8            [ 4]  259     cp  b
   5245 20 06         [12]  260     jr nz, _48bit4
   5247 1E 41         [ 7]  261     ld  e, #65
   5249 21 08 20      [10]  262     ld  hl, #Key_A
   524C C9            [10]  263     ret
   524D                     264 _48bit4:
   524D CB 08         [ 8]  265     rrc b
   524F B8            [ 4]  266     cp  b
   5250 20 06         [12]  267     jr nz, _48bit3
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 24.
Hexadecimal [16-Bits]



   5252 1E 3E         [ 7]  268     ld  e, #62
   5254 21 08 10      [10]  269     ld  hl, #Key_Tab
   5257 C9            [10]  270     ret
   5258                     271 _48bit3:
   5258 CB 08         [ 8]  272     rrc b
   525A B8            [ 4]  273     cp  b
   525B 20 06         [12]  274     jr nz, _48bit2
   525D 1E 51         [ 7]  275     ld  e, #81
   525F 21 08 08      [10]  276     ld  hl, #Key_Q
   5262 C9            [10]  277     ret
   5263                     278 _48bit2:
   5263 CB 08         [ 8]  279     rrc b
   5265 B8            [ 4]  280     cp  b
   5266 20 06         [12]  281     jr nz, _48bit1
   5268 1E 7F         [ 7]  282     ld  e, #127
   526A 21 08 04      [10]  283     ld  hl, #Key_Esc
   526D C9            [10]  284     ret
   526E                     285 _48bit1:
   526E CB 08         [ 8]  286     rrc b
   5270 B8            [ 4]  287     cp  b
   5271 20 06         [12]  288     jr nz, _48bit0
   5273 1E 32         [ 7]  289     ld  e, #50
   5275 21 08 02      [10]  290     ld  hl, #Key_2
   5278 C9            [10]  291     ret
   5279                     292 _48bit0:
   5279 CB 08         [ 8]  293     rrc b
   527B B8            [ 4]  294     cp  b
   527C C0            [11]  295     ret nz
   527D 1E 31         [ 7]  296     ld  e, #49
   527F 21 08 01      [10]  297     ld  hl, #Key_1
   5282 C9            [10]  298     ret
   5283                     299 _47:
   5283 F1            [10]  300     pop af                          ;; Recupero tecla pulsada
   5284 06 7F         [ 7]  301     ld  b, #0x7F                    
   5286 B8            [ 4]  302     cp  b
   5287 20 06         [12]  303     jr  nz, _47bit6
   5289 1E 58         [ 7]  304     ld  e, #88
   528B 21 07 80      [10]  305     ld  hl, #Key_X
   528E C9            [10]  306     ret
   528F                     307 _47bit6:
   528F CB 08         [ 8]  308     rrc b
   5291 B8            [ 4]  309     cp  b
   5292 20 06         [12]  310     jr nz, _47bit5
   5294 1E 43         [ 7]  311     ld  e, #67
   5296 21 07 40      [10]  312     ld  hl, #Key_C
   5299 C9            [10]  313     ret
   529A                     314 _47bit5:
   529A CB 08         [ 8]  315     rrc b
   529C B8            [ 4]  316     cp  b
   529D 20 06         [12]  317     jr nz, _47bit4
   529F 1E 44         [ 7]  318     ld  e, #68
   52A1 21 07 20      [10]  319     ld  hl, #Key_D
   52A4 C9            [10]  320     ret
   52A5                     321 _47bit4:
   52A5 CB 08         [ 8]  322     rrc b
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 25.
Hexadecimal [16-Bits]



   52A7 B8            [ 4]  323     cp  b
   52A8 20 06         [12]  324     jr nz, _47bit3
   52AA 1E 53         [ 7]  325     ld  e, #83
   52AC 21 07 10      [10]  326     ld  hl, #Key_S
   52AF C9            [10]  327     ret
   52B0                     328 _47bit3:
   52B0 CB 08         [ 8]  329     rrc b
   52B2 B8            [ 4]  330     cp  b
   52B3 20 06         [12]  331     jr nz, _47bit2
   52B5 1E 57         [ 7]  332     ld  e, #87
   52B7 21 07 08      [10]  333     ld  hl, #Key_W
   52BA C9            [10]  334     ret
   52BB                     335 _47bit2:
   52BB CB 08         [ 8]  336     rrc b
   52BD B8            [ 4]  337     cp  b
   52BE 20 06         [12]  338     jr nz, _47bit1
   52C0 1E 45         [ 7]  339     ld  e, #69
   52C2 21 07 04      [10]  340     ld  hl, #Key_E
   52C5 C9            [10]  341     ret
   52C6                     342 _47bit1:
   52C6 CB 08         [ 8]  343     rrc b
   52C8 B8            [ 4]  344     cp  b
   52C9 20 06         [12]  345     jr nz, _47bit0
   52CB 1E 33         [ 7]  346     ld  e, #51
   52CD 21 07 02      [10]  347     ld  hl, #Key_3
   52D0 C9            [10]  348     ret
   52D1                     349 _47bit0:
   52D1 CB 08         [ 8]  350     rrc b
   52D3 B8            [ 4]  351     cp  b
   52D4 C0            [11]  352     ret nz
   52D5 1E 34         [ 7]  353     ld  e, #52
   52D7 21 07 01      [10]  354     ld  hl, #Key_4
   52DA C9            [10]  355     ret
   52DB                     356 _46:
   52DB F1            [10]  357     pop af                          ;; Recupero tecla pulsada
   52DC 06 7F         [ 7]  358     ld  b, #0x7F                    
   52DE B8            [ 4]  359     cp  b
   52DF 20 06         [12]  360     jr  nz, _46bit6
   52E1 1E 56         [ 7]  361     ld  e, #86
   52E3 21 06 80      [10]  362     ld  hl, #Key_V
   52E6 C9            [10]  363     ret
   52E7                     364 _46bit6:
   52E7 CB 08         [ 8]  365     rrc b
   52E9 B8            [ 4]  366     cp  b
   52EA 20 06         [12]  367     jr nz, _46bit5
   52EC 1E 42         [ 7]  368     ld  e, #66
   52EE 21 06 40      [10]  369     ld  hl, #Key_B
   52F1 C9            [10]  370     ret
   52F2                     371 _46bit5:
   52F2 CB 08         [ 8]  372     rrc b
   52F4 B8            [ 4]  373     cp  b
   52F5 20 06         [12]  374     jr nz, _46bit4
   52F7 1E 46         [ 7]  375     ld  e, #70
   52F9 21 06 20      [10]  376     ld  hl, #Key_F
   52FC C9            [10]  377     ret
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 26.
Hexadecimal [16-Bits]



   52FD                     378 _46bit4:
   52FD CB 08         [ 8]  379     rrc b
   52FF B8            [ 4]  380     cp  b
   5300 20 06         [12]  381     jr nz, _46bit3
   5302 1E 47         [ 7]  382     ld  e, #71
   5304 21 06 10      [10]  383     ld  hl, #Key_G
   5307 C9            [10]  384     ret
   5308                     385 _46bit3:
   5308 CB 08         [ 8]  386     rrc b
   530A B8            [ 4]  387     cp  b
   530B C2 14 53      [10]  388     jp nz, _46bit2
   530E 1E 54         [ 7]  389     ld  e, #84
   5310 21 06 08      [10]  390     ld  hl, #Key_T
   5313 C9            [10]  391     ret
   5314                     392 _46bit2:
   5314 CB 08         [ 8]  393     rrc b
   5316 B8            [ 4]  394     cp  b
   5317 20 06         [12]  395     jr nz, _46bit1
   5319 1E 52         [ 7]  396     ld  e, #82
   531B 21 06 04      [10]  397     ld  hl, #Key_R
   531E C9            [10]  398     ret
   531F                     399 _46bit1:
   531F CB 08         [ 8]  400     rrc b
   5321 B8            [ 4]  401     cp  b
   5322 20 06         [12]  402     jr nz, _46bit0
   5324 1E 35         [ 7]  403     ld  e, #53
   5326 21 06 02      [10]  404     ld  hl, #Key_5
   5329 C9            [10]  405     ret
   532A                     406 _46bit0:
   532A CB 08         [ 8]  407     rrc b
   532C B8            [ 4]  408     cp  b
   532D C0            [11]  409     ret nz
   532E 1E 36         [ 7]  410     ld  e, #54
   5330 21 06 01      [10]  411     ld  hl, #Key_6
   5333 C9            [10]  412     ret
   5334                     413 _45:
   5334 F1            [10]  414     pop af                          ;; Recupero tecla pulsada
   5335 06 7F         [ 7]  415     ld  b, #0x7F                    
   5337 B8            [ 4]  416     cp  b
   5338 20 06         [12]  417     jr  nz, _45bit6
   533A 1E 7F         [ 7]  418     ld  e, #127
   533C 21 05 80      [10]  419     ld  hl, #Key_Space
   533F C9            [10]  420     ret
   5340                     421 _45bit6:
   5340 CB 08         [ 8]  422     rrc b
   5342 B8            [ 4]  423     cp  b
   5343 20 06         [12]  424     jr nz, _45bit5
   5345 1E 4E         [ 7]  425     ld  e, #78
   5347 21 05 40      [10]  426     ld  hl, #Key_N
   534A C9            [10]  427     ret
   534B                     428 _45bit5:
   534B CB 08         [ 8]  429     rrc b
   534D B8            [ 4]  430     cp  b
   534E 20 06         [12]  431     jr nz, _45bit4
   5350 1E 4A         [ 7]  432     ld  e, #74
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 27.
Hexadecimal [16-Bits]



   5352 21 05 20      [10]  433     ld  hl, #Key_J
   5355 C9            [10]  434     ret
   5356                     435 _45bit4:
   5356 CB 08         [ 8]  436     rrc b
   5358 B8            [ 4]  437     cp  b
   5359 20 06         [12]  438     jr nz, _45bit3
   535B 1E 48         [ 7]  439     ld  e, #72
   535D 21 05 10      [10]  440     ld  hl, #Key_H
   5360 C9            [10]  441     ret
   5361                     442 _45bit3:
   5361 CB 08         [ 8]  443     rrc b
   5363 B8            [ 4]  444     cp  b
   5364 20 06         [12]  445     jr nz, _45bit2
   5366 1E 59         [ 7]  446     ld  e, #89
   5368 21 05 08      [10]  447     ld  hl, #Key_Y
   536B C9            [10]  448     ret
   536C                     449 _45bit2:
   536C CB 08         [ 8]  450     rrc b
   536E B8            [ 4]  451     cp  b
   536F 20 06         [12]  452     jr nz, _45bit1
   5371 1E 55         [ 7]  453     ld  e, #85
   5373 21 05 04      [10]  454     ld  hl, #Key_U
   5376 C9            [10]  455     ret
   5377                     456 _45bit1:
   5377 CB 08         [ 8]  457     rrc b
   5379 B8            [ 4]  458     cp  b
   537A 20 06         [12]  459     jr nz, _45bit0
   537C 1E 37         [ 7]  460     ld  e, #55
   537E 21 05 02      [10]  461     ld  hl, #Key_7
   5381 C9            [10]  462     ret
   5382                     463 _45bit0:
   5382 CB 08         [ 8]  464     rrc b
   5384 B8            [ 4]  465     cp  b
   5385 C0            [11]  466     ret nz
   5386 1E 38         [ 7]  467     ld  e, #56
   5388 21 05 01      [10]  468     ld  hl, #Key_8
   538B C9            [10]  469     ret
   538C                     470 _44:
   538C F1            [10]  471     pop af                          ;; Recupero tecla pulsada
   538D 06 7F         [ 7]  472     ld  b, #0x7F                    
   538F B8            [ 4]  473     cp  b
   5390 20 06         [12]  474     jr  nz, _44bit6
   5392 1E 2C         [ 7]  475     ld  e, #44
   5394 21 04 80      [10]  476     ld  hl, #Key_Comma
   5397 C9            [10]  477     ret
   5398                     478 _44bit6:
   5398 CB 08         [ 8]  479     rrc b
   539A B8            [ 4]  480     cp  b
   539B 20 06         [12]  481     jr nz, _44bit5
   539D 1E 4D         [ 7]  482     ld  e, #77
   539F 21 04 40      [10]  483     ld  hl, #Key_M
   53A2 C9            [10]  484     ret
   53A3                     485 _44bit5:
   53A3 CB 08         [ 8]  486     rrc b
   53A5 B8            [ 4]  487     cp  b
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 28.
Hexadecimal [16-Bits]



   53A6 20 06         [12]  488     jr nz, _44bit4
   53A8 1E 4B         [ 7]  489     ld  e, #75
   53AA 21 04 20      [10]  490     ld  hl, #Key_K
   53AD C9            [10]  491     ret
   53AE                     492 _44bit4:
   53AE CB 08         [ 8]  493     rrc b
   53B0 B8            [ 4]  494     cp  b
   53B1 20 06         [12]  495     jr nz, _44bit3
   53B3 1E 4C         [ 7]  496     ld  e, #76
   53B5 21 04 10      [10]  497     ld  hl, #Key_L
   53B8 C9            [10]  498     ret
   53B9                     499 _44bit3:
   53B9 CB 08         [ 8]  500     rrc b
   53BB B8            [ 4]  501     cp  b
   53BC 20 06         [12]  502     jr nz, _44bit2
   53BE 1E 49         [ 7]  503     ld  e, #73
   53C0 21 04 08      [10]  504     ld  hl, #Key_I
   53C3 C9            [10]  505     ret
   53C4                     506 _44bit2:
   53C4 CB 08         [ 8]  507     rrc b
   53C6 B8            [ 4]  508     cp  b
   53C7 20 06         [12]  509     jr nz, _44bit1
   53C9 1E 4F         [ 7]  510     ld  e, #79
   53CB 21 04 04      [10]  511     ld  hl, #Key_O
   53CE C9            [10]  512     ret
   53CF                     513 _44bit1:
   53CF CB 08         [ 8]  514     rrc b
   53D1 B8            [ 4]  515     cp  b
   53D2 20 06         [12]  516     jr nz, _44bit0
   53D4 1E 39         [ 7]  517     ld  e, #57
   53D6 21 04 02      [10]  518     ld  hl, #Key_9
   53D9 C9            [10]  519     ret
   53DA                     520 _44bit0:
   53DA CB 08         [ 8]  521     rrc b
   53DC B8            [ 4]  522     cp  b
   53DD C0            [11]  523     ret nz
   53DE 1E 30         [ 7]  524     ld  e, #48
   53E0 21 04 01      [10]  525     ld  hl, #Key_0
   53E3 C9            [10]  526     ret
   53E4                     527 _43:
   53E4 F1            [10]  528     pop af                          ;; Recupero tecla pulsada
   53E5 06 7F         [ 7]  529     ld  b, #0x7F                    
   53E7 B8            [ 4]  530     cp  b
   53E8 20 06         [12]  531     jr  nz, _43bit6
   53EA 1E 2E         [ 7]  532     ld  e, #46
   53EC 21 03 80      [10]  533     ld  hl, #Key_Dot
   53EF C9            [10]  534     ret
   53F0                     535 _43bit6:
   53F0 CB 08         [ 8]  536     rrc b
   53F2 B8            [ 4]  537     cp  b
   53F3 20 06         [12]  538     jr nz, _43bit5
   53F5 1E 2F         [ 7]  539     ld  e, #47
   53F7 21 03 40      [10]  540     ld  hl, #Key_Slash
   53FA C9            [10]  541     ret
   53FB                     542 _43bit5:
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 29.
Hexadecimal [16-Bits]



   53FB CB 08         [ 8]  543     rrc b
   53FD B8            [ 4]  544     cp  b
   53FE 20 06         [12]  545     jr nz, _43bit4
   5400 1E 3A         [ 7]  546     ld  e, #58
   5402 21 03 20      [10]  547     ld  hl, #Key_Colon
   5405 C9            [10]  548     ret
   5406                     549 _43bit4:
   5406 CB 08         [ 8]  550     rrc b
   5408 B8            [ 4]  551     cp  b
   5409 20 06         [12]  552     jr nz, _43bit3
   540B 1E 3B         [ 7]  553     ld  e, #59
   540D 21 03 10      [10]  554     ld  hl, #Key_SemiColon
   5410 C9            [10]  555     ret
   5411                     556 _43bit3:
   5411 CB 08         [ 8]  557     rrc b
   5413 B8            [ 4]  558     cp  b
   5414 20 06         [12]  559     jr nz, _43bit2
   5416 1E 50         [ 7]  560     ld  e, #80
   5418 21 03 08      [10]  561     ld  hl, #Key_P
   541B C9            [10]  562     ret
   541C                     563 _43bit2:
   541C CB 08         [ 8]  564     rrc b
   541E B8            [ 4]  565     cp  b
   541F 20 06         [12]  566     jr nz, _43bit1
   5421 1E 40         [ 7]  567     ld  e, #64
   5423 21 03 04      [10]  568     ld  hl, #Key_At
   5426 C9            [10]  569     ret
   5427                     570 _43bit1:
   5427 CB 08         [ 8]  571     rrc b
   5429 B8            [ 4]  572     cp  b
   542A 20 06         [12]  573     jr nz, _43bit0
   542C 1E 2D         [ 7]  574     ld  e, #45
   542E 21 03 02      [10]  575     ld  hl, #Key_Hyphen
   5431 C9            [10]  576     ret
   5432                     577 _43bit0:
   5432 CB 08         [ 8]  578     rrc b
   5434 B8            [ 4]  579     cp  b
   5435 C0            [11]  580     ret nz
   5436 1E A0         [ 7]  581     ld  e, #160
   5438 21 03 01      [10]  582     ld  hl, #Key_Caret
   543B C9            [10]  583     ret
   543C                     584 _42:
   543C F1            [10]  585     pop af                          ;; Recupero tecla pulsada
   543D 06 7F         [ 7]  586     ld  b, #0x7F                    
   543F B8            [ 4]  587     cp  b
   5440 20 06         [12]  588     jr  nz, _42bit6
   5442 1E 7F         [ 7]  589     ld  e, #127
   5444 21 02 80      [10]  590     ld  hl, #Key_Control
   5447 C9            [10]  591     ret
   5448                     592 _42bit6:
   5448 CB 08         [ 8]  593     rrc b
   544A B8            [ 4]  594     cp  b
   544B 20 06         [12]  595     jr nz, _42bit5
   544D 1E 5C         [ 7]  596     ld  e, #92
   544F 21 02 40      [10]  597     ld  hl, #Key_BackSlash
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 30.
Hexadecimal [16-Bits]



   5452 C9            [10]  598     ret
   5453                     599 _42bit5:
   5453 CB 08         [ 8]  600     rrc b
   5455 B8            [ 4]  601     cp  b
   5456 20 06         [12]  602     jr nz, _42bit4
   5458 1E 86         [ 7]  603     ld  e, #134
   545A 21 02 20      [10]  604     ld  hl, #Key_Shift
   545D C9            [10]  605     ret
   545E                     606 _42bit4:
   545E CB 08         [ 8]  607     rrc b
   5460 B8            [ 4]  608     cp  b
   5461 20 06         [12]  609     jr nz, _42bit3
   5463 1E 83         [ 7]  610     ld  e, #131
   5465 21 02 10      [10]  611     ld  hl, #Key_F4
   5468 C9            [10]  612     ret
   5469                     613 _42bit3:
   5469 CB 08         [ 8]  614     rrc b
   546B B8            [ 4]  615     cp  b
   546C 20 06         [12]  616     jr nz, _42bit2
   546E 1E 5D         [ 7]  617     ld  e, #93
   5470 21 02 08      [10]  618     ld  hl, #Key_CloseBracket
   5473 C9            [10]  619     ret
   5474                     620 _42bit2:
   5474 CB 08         [ 8]  621     rrc b
   5476 B8            [ 4]  622     cp  b
   5477 20 06         [12]  623     jr nz, _42bit1
   5479 1E 60         [ 7]  624     ld  e, #96
   547B 21 02 04      [10]  625     ld  hl, #Key_Return
   547E C9            [10]  626     ret
   547F                     627 _42bit1:
   547F CB 08         [ 8]  628     rrc b
   5481 B8            [ 4]  629     cp  b
   5482 20 06         [12]  630     jr nz, _42bit0
   5484 1E 5B         [ 7]  631     ld  e, #91
   5486 21 02 02      [10]  632     ld  hl, #Key_OpenBracket
   5489 C9            [10]  633     ret
   548A                     634 _42bit0:
   548A CB 08         [ 8]  635     rrc b
   548C B8            [ 4]  636     cp  b
   548D C0            [11]  637     ret nz
   548E 1E FD         [ 7]  638     ld  e, #253
   5490 21 02 01      [10]  639     ld  hl, #Key_Clr
   5493 C9            [10]  640     ret
   5494                     641 _41:
   5494 F1            [10]  642     pop af                          ;; Recupero tecla pulsada
   5495 06 7F         [ 7]  643     ld  b, #0x7F                    
   5497 B8            [ 4]  644     cp  b
   5498 20 06         [12]  645     jr  nz, _41bit6
   549A 1E 82         [ 7]  646     ld  e, #130
   549C 21 01 80      [10]  647     ld  hl, #Key_F0
   549F C9            [10]  648     ret
   54A0                     649 _41bit6:
   54A0 CB 08         [ 8]  650     rrc b
   54A2 B8            [ 4]  651     cp  b
   54A3 20 06         [12]  652     jr nz, _41bit5
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 31.
Hexadecimal [16-Bits]



   54A5 1E 84         [ 7]  653     ld  e, #132
   54A7 21 01 40      [10]  654     ld  hl, #Key_F2
   54AA C9            [10]  655     ret
   54AB                     656 _41bit5:
   54AB CB 08         [ 8]  657     rrc b
   54AD B8            [ 4]  658     cp  b
   54AE 20 06         [12]  659     jr nz, _41bit4
   54B0 1E 83         [ 7]  660     ld  e, #131
   54B2 21 01 20      [10]  661     ld  hl, #Key_F1
   54B5 C9            [10]  662     ret
   54B6                     663 _41bit4:
   54B6 CB 08         [ 8]  664     rrc b
   54B8 B8            [ 4]  665     cp  b
   54B9 20 06         [12]  666     jr nz, _41bit3
   54BB 1E 87         [ 7]  667     ld  e, #135
   54BD 21 01 10      [10]  668     ld  hl, #Key_F5
   54C0 C9            [10]  669     ret
   54C1                     670 _41bit3:
   54C1 CB 08         [ 8]  671     rrc b
   54C3 B8            [ 4]  672     cp  b
   54C4 20 06         [12]  673     jr nz, _41bit2
   54C6 1E 8A         [ 7]  674     ld  e, #138
   54C8 21 01 08      [10]  675     ld  hl, #Key_F8
   54CB C9            [10]  676     ret
   54CC                     677 _41bit2:
   54CC CB 08         [ 8]  678     rrc b
   54CE B8            [ 4]  679     cp  b
   54CF 20 06         [12]  680     jr nz, _41bit1
   54D1 1E 89         [ 7]  681     ld  e, #137
   54D3 21 01 04      [10]  682     ld  hl, #Key_F7
   54D6 C9            [10]  683     ret
   54D7                     684 _41bit1:
   54D7 CB 08         [ 8]  685     rrc b
   54D9 B8            [ 4]  686     cp  b
   54DA 20 06         [12]  687     jr nz, _41bit0
   54DC 1E 93         [ 7]  688     ld  e, #147
   54DE 21 01 02      [10]  689     ld  hl, #Key_Copy
   54E1 C9            [10]  690     ret
   54E2                     691 _41bit0:
   54E2 CB 08         [ 8]  692     rrc b
   54E4 B8            [ 4]  693     cp  b
   54E5 C0            [11]  694     ret nz
   54E6 1E F2         [ 7]  695     ld  e, #242
   54E8 21 01 01      [10]  696     ld  hl, #Key_CursorLeft
   54EB C9            [10]  697     ret
   54EC                     698 _40:
   54EC F1            [10]  699     pop af                          ;; Recupero tecla pulsada
   54ED 06 7F         [ 7]  700     ld  b, #0x7F                    
   54EF B8            [ 4]  701     cp  b
   54F0 20 06         [12]  702     jr  nz, _40bit6
   54F2 1E 90         [ 7]  703     ld  e, #144
   54F4 21 00 80      [10]  704     ld  hl, #Key_FDot
   54F7 C9            [10]  705     ret
   54F8                     706 _40bit6:
   54F8 CB 08         [ 8]  707     rrc b
ASxxxx Assembler V02.00 + NoICE + SDCC mods  (Zilog Z80 / Hitachi HD64180), page 32.
Hexadecimal [16-Bits]



   54FA B8            [ 4]  708     cp  b
   54FB 20 06         [12]  709     jr nz, _40bit5
   54FD 1E E9         [ 7]  710     ld  e, #233
   54FF 21 00 40      [10]  711     ld  hl, #Key_Enter
   5502 C9            [10]  712     ret
   5503                     713 _40bit5:
   5503 CB 08         [ 8]  714     rrc b
   5505 B8            [ 4]  715     cp  b
   5506 20 06         [12]  716     jr nz, _40bit4
   5508 1E 85         [ 7]  717     ld  e, #133
   550A 21 00 20      [10]  718     ld  hl, #Key_F3
   550D C9            [10]  719     ret
   550E                     720 _40bit4:
   550E CB 08         [ 8]  721     rrc b
   5510 B8            [ 4]  722     cp  b
   5511 20 06         [12]  723     jr nz, _40bit3
   5513 1E 88         [ 7]  724     ld  e, #136
   5515 21 00 10      [10]  725     ld  hl, #Key_F6
   5518 C9            [10]  726     ret
   5519                     727 _40bit3:
   5519 CB 08         [ 8]  728     rrc b
   551B B8            [ 4]  729     cp  b
   551C 20 06         [12]  730     jr nz, _40bit2
   551E 1E 8B         [ 7]  731     ld  e, #139
   5520 21 00 08      [10]  732     ld  hl, #Key_F9
   5523 C9            [10]  733     ret
   5524                     734 _40bit2:
   5524 CB 08         [ 8]  735     rrc b
   5526 B8            [ 4]  736     cp  b
   5527 20 06         [12]  737     jr nz, _40bit1
   5529 1E F1         [ 7]  738     ld  e, #241
   552B 21 00 04      [10]  739     ld  hl, #Key_CursorDown
   552E C9            [10]  740     ret
   552F                     741 _40bit1:
   552F CB 08         [ 8]  742     rrc b
   5531 B8            [ 4]  743     cp  b
   5532 20 06         [12]  744     jr nz, _40bit0
   5534 1E F3         [ 7]  745     ld  e, #243
   5536 21 00 02      [10]  746     ld  hl, #Key_CursorRight
   5539 C9            [10]  747     ret
   553A                     748 _40bit0:
   553A CB 08         [ 8]  749     rrc b
   553C B8            [ 4]  750     cp  b
   553D C0            [11]  751     ret nz
   553E 1E F0         [ 7]  752     ld  e, #240
   5540 21 00 01      [10]  753     ld  hl, #Key_CursorUp
   5543 C9            [10]  754     ret
