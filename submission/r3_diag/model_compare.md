# L / R / M

L/R/M là thứ tự box in-scope theo XML của từng frame.

## adasind_001320.jpg
- L1+R2+M4: LRM (center)
- L3+R4+M1: LRM (edge)
- L6+R1+M2: LRM (center)
- L5+R6: LR_noM (edge)
- L2+R3: LR_noM (mid)
- L4+R5: LR_noM (mid)
- M3: M_only (center)
- M5: M_only (mid)
- M6: M_only (edge)
## adasind_014670.jpg
- L3+R1+M1: LRM (edge)
- L5+R3: LR_noM (mid)
- L7+R4+M4: LRM (center)
- L1+R2+M5: LRM (center)
- L2: L_only (center)
- L4+M3: LM_noR (mid)
- L6: L_only (center)
- R5: R_only (mid)
- M6: M_only (mid)
- M7: M_only (center)
## adasind_034080.jpg
- L3+R3+M5: LRM (edge)
- L5+R5: LR_noM (mid)
- L4+R1+M1: LRM (mid)
- L1+R6+M2: LRM (mid)
- L7+R7: LR_noM (center)
- L2+R4+M4: LRM (mid)
- L8+R9: LR_noM (center)
- L6+R8: LR_noM (mid)
- L9: L_only (center)
- L10+M3: LM_noR (center)
- R2: R_only (center)
- M7: M_only (mid)
- M8: M_only (mid)
- M9: M_only (mid)
- M10: M_only (center)
- M11: M_only (center)
- M12: M_only (mid)

## Zone × cell
| zone | LRM | LR_noM | LM_noR | L_only | RM_noL | R_only | M_only |
|---|---|---|---|---|---|---|---|
| center | 4 | 2 | 1 | 3 | 0 | 1 | 4 |
| mid | 3 | 5 | 1 | 0 | 0 | 1 | 6 |
| edge | 3 | 1 | 0 | 0 | 0 | 0 | 1 |
