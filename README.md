# SAP
#add field and custom logic for app "Monitor production order report" (Tcode: COOIS)

![image](https://github.com/Huy-Hieu/SAP/assets/64250806/f1272060-51ee-43d4-8b05-93a3e4afe5e5)

![image](https://github.com/Huy-Hieu/SAP/assets/64250806/cc0e214a-47b1-4593-a148-21dcecb57cfe)

![image](https://github.com/Huy-Hieu/SAP/assets/64250806/92194d41-a68a-4bae-89de-3bf0f54700c6)

![image](https://github.com/Huy-Hieu/SAP/assets/64250806/795c2aa0-dbf0-4da0-bdf1-1d2d5b2029be)

#code ex:
    SELECT
        ManufacturingOrder,
        ProductConfiguration,
        ZTHICKNESS1,
        ZTHICKNESS1_UNIT,
        ZLENGTH1,
        ZLENGTH1_UNIT,
        ZMETHOD,
        ZWIDTH1,
        ZWIDTH1_UNIT,
        ZSTEEL_TYPE
    FROM YY1_Productionvariant( P_KEYDATE = '20240101',
                               P_LANGUAGE = 'E' ) WITH PRIVILEGED ACCESS AS data
    INTO table @DATA(lt_result).
    Sort lt_result by ManufacturingOrder.
    Read table lt_result into data(ls_result) with key  ManufacturingOrder = manufacturingorder-manufacturingorder binary search .
    if sy-subrc = 0.
        manufacturingorder_changed-yy1_spec_ord = ls_result-zsteel_type.
        manufacturingorder_changed-yy1_cuttingmethod_ord = ls_result-zmethod.

        manufacturingorder_changed-yy1_thickness_ord = ls_result-zthickness1.
        manufacturingorder_changed-yy1_thickness_ordu = ls_result-zthickness1_unit.

        manufacturingorder_changed-yy1_length_ord  = ls_result-zlength1.
        manufacturingorder_changed-yy1_length_ordu = ls_result-zlength1_unit.

        manufacturingorder_changed-yy1_width_ord   = ls_result-zwidth1.
        manufacturingorder_changed-yy1_width_ordu  = ls_result-zwidth1_unit.

    "Activate changes by providing internal key
    manufacturingorder_changed-manufacturingorder = manufacturingorder-manufacturingorder.
    endif.






