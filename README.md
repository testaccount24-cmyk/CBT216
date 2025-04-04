# CBT216
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. GitHub repos will be deleted and created during this period...

```
//***FILE 216 IS FROM JIM SMITH OF NATIONAL LINEN SERVICE IN        *   FILE 216
//*           ATLANTA, GEORGIA.  THIS FILE CONTAINS A GENERAL       *   FILE 216
//*           DATE MANIPULATION ROUTINE WHICH HAS A SIZABLE         *   FILE 216
//*           VARIETY OF SEPARATE FUNCTIONS.  DESCRIPTION IS        *   FILE 216
//*           BELOW.                                                *   FILE 216
//*                                                                 *   FILE 216
//*     PROGRAM: CNVDATE                                            *   FILE 216
//*     PURPOSE: DATE CONVERSION AND MANIPULATION                   *   FILE 216
//*                                                                 *   FILE 216
//*     ORIGINAL AUTHOR: WES CLEVELAND                              *   FILE 216
//*              NATIONAL SERVICE INDUSTRIES                        *   FILE 216
//*              INFORMATION SYSTEMS                                *   FILE 216
//*                                                                 *   FILE 216
//*     CONTRIBUTOR:                                                *   FILE 216
//*                                                                 *   FILE 216
//*              JIM SMITH                                          *   FILE 216
//*              NATIONAL SERVICE INDUSTRIES                        *   FILE 216
//*              INFORMATION SYSTEMS                                *   FILE 216
//*              MAIL STOP 003                                      *   FILE 216
//*              1420 PEACHTREE ST N.E.                             *   FILE 216
//*              ATLANTA, GEORGIA  30309                            *   FILE 216
//*              (404) 853-6434    WORK                             *   FILE 216
//*                                                                 *   FILE 216
//*   -----------------------------------------------------------   *   FILE 216
//*                                                                 *   FILE 216
//*        FUNCTION:                                                *   FILE 216
//*                                                                 *   FILE 216
//*        THIS PROGRAM IS A GENERAL PURPOSE DATE MANIPULATION      *   FILE 216
//*        ROUTINE THAT MAY BE CALLED TO PERFORM THE FOLLOWING      *   FILE 216
//*        DATE MANIPULATION FUNCTIONS:                             *   FILE 216
//*                                                                 *   FILE 216
//*        1)   VERIFY    JULIAN & GREGORIAN DATES                  *   FILE 216
//*        2)   CONVERT   JULIAN & GREGORIAN DATES                  *   FILE 216
//*        3)   INCREMENT JULIAN & GREGORIAN DATES                  *   FILE 216
//*        4)   DECREMENT JULIAN & GREGORIAN DATES                  *   FILE 216
//*        5)   CALCULATE DIFFERENCE BETWEEN JULIAN OR              *   FILE 216
//*               GREGORIAN DATES                                   *   FILE 216
//*        6)   CALCULATE DAY OF WEEK                               *   FILE 216
//*        7)   CALCULATE DAY OF CENTURY                            *   FILE 216
//*                                                                 *   FILE 216
//*        LINKAGE:  R1 = CNVDATE WORK AREA ADDRESS                 *   FILE 216
//*                 R13 = SAVE AREA ADDRESS                         *   FILE 216
//*                 R14 = RETURN ADDRESS                            *   FILE 216
//*                 R15 = ENTRY ADDRESS                             *   FILE 216
//*                                                                 *   FILE 216
//*         RETURN: R15 = RETURN CODE                               *   FILE 216
//*                       00 = FUNCTION COMPLETE WITHOUT ERROR      *   FILE 216
//*                       04 = INVALID DATE DATA                    *   FILE 216
//*                       08 = INVALID PARAMATER SPECIFICATION      *   FILE 216
//*                                                                 *   FILE 216
//*         FORMATS: GREGORIAN - MMDDYY                             *   FILE 216
//*                              MMDDYYYY                           *   FILE 216
//*                     JULIAN - YYDDD                              *   FILE 216
//*                              YYYYDDD                            *   FILE 216
//*                      VALUE - DDDD                               *   FILE 216
//*                              DDDDDDDD                           *   FILE 216
//*                        DAY - DXXXXXXXXX (D=DAY NUMBER,          *   FILE 216
//*                               X=DAY SPELLED OUT)                *   FILE 216
//*                                                                 *   FILE 216
//*         NOTE: INITIALIZE FIELDS-1 AND FIELD-2 WITH BLANKS       *   FILE 216
//*               BEFORE MOVING IN REQUESTED DATES (FIELD-1) OR     *   FILE 216
//*               INCREMENT/DECREMENT NBR (FIELDS-2).  THE          *   FILE 216
//*               INCREMENT/DECREMENT NBR (FIELD-2) MUST BE LEFT    *   FILE 216
//*               JUSTIFIED.                                        *   FILE 216
//*                                                                 *   FILE 216
//*           WORK AREA - FUNCTION  (1 BYTE)                        *   FILE 216
//*                       FIELD-1   (8 BYTES)                       *   FILE 216
//*                       FIELD-2   (8 BYTES)                       *   FILE 216
//*                       RETURN   (10 BYTES)                       *   FILE 216
//*                                                                 *   FILE 216
//*                       FUNC    FIELD-1    FIELD-2    RETURN      *   FILE 216
//*                       ----    -------    -------    -------     *   FILE 216
//*                         1     DATE       N/A        N/A         *   FILE 216
//*                         2     DATE       N/A        DATE        *   FILE 216
//*                         3     DATE       DDDDDDDD   DATE        *   FILE 216
//*                         4     DATE       DDDDDDDD   DATE        *   FILE 216
//*                         5     DATE       DATE       DDDDDDDD    *   FILE 216
//*                         6     DATE       N/A        DXXXXXXXXX  *   FILE 216
//*                         7     DATE       N/A        DDDDDDDD    *   FILE 216
//*                                                                 *   FILE 216
//*        EXAMPLE COBOL: CURRENT-DATE 071392 DECREMENT 30 DAYS     *   FILE 216
//*                                                                 *   FILE 216
//*             01  WS-CNVDATE-WORK-AREA    PIC X(27).              *   FILE 216
//*                                                                 *   FILE 216
//*             01  FILLER REDEFINES WS-CNVDATE-WORK-AREA.          *   FILE 216
//*                                                                 *   FILE 216
//*                 05  FUNCTION            PIC X.                  *   FILE 216
//*                 05  SUBR-DATE.                                  *   FILE 216
//*                     10  SR-MM           PIC XX.                 *   FILE 216
//*                     10  SR-DD           PIC XX.                 *   FILE 216
//*                     10  SR-YY           PIC XX.                 *   FILE 216
//*                     10  FILLER          PIC XX.                 *   FILE 216
//*                 05  AGE-CRITERA         PIC X(8).               *   FILE 216
//*                 05  AGED-DATE.                                  *   FILE 216
//*                     10  AD-MM           PIC XX.                 *   FILE 216
//*                     10  AD-DD           PIC XX.                 *   FILE 216
//*                     10  AD-YY           PIC XX.                 *   FILE 216
//*                     10  FILLER          PIC XXXX.               *   FILE 216
//*                                                                 *   FILE 216
//*                                                                 *   FILE 216
//*                 MOVE CURRENT-DATE TO WORK-DATE.                 *   FILE 216
//*                 MOVE SPACES TO WS-CNVDATE-WORK-AREA.            *   FILE 216
//*                 MOVE WD-MM TO SR-MM.                            *   FILE 216
//*                 MOVE WD-DD TO SR-DD.                            *   FILE 216
//*                 MOVE WD-YY TO SR-YY.                            *   FILE 216
//*                 MOVE '4' TO FUNCTION.                           *   FILE 216
//*                 MOVE '30      ' TO AGE-CRITERA.                 *   FILE 216
//*                 CALL 'CNVDATE' USING WS-CNVDATE-WORK-AREA.      *   FILE 216
//*                                                                 *   FILE 216
//*        EXAMPLE RETURNED AGED-DATE: 061392                       *   FILE 216
//*                                                                 *   FILE 216
```
