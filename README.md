NUMBERED_DATA AS (
    SELECT
        G.*,
        ROW_NUMBER() OVER (
            PARTITION BY
                INST_CD,
                CODE_PORTEFEUILLE,
                COTATION_DT,
                COURS,
                COURS_DTE,
                VALO_DT
            ORDER BY PX_NAV
        ) AS RN
    FROM GROUPED_DATA G
)

SELECT
    CODE_PORTEFEUILLE,
    INST_CD,
    COTATION_DT,
    COURS,
    COURS_DTE,
    VALO_DT,

    MAX(CASE WHEN RN = 1 THEN PX_NAV END) AS PX_NAV_1,
    MAX(CASE WHEN RN = 1 THEN LIBS END)   AS LIBS_1,

    MAX(CASE WHEN RN = 2 THEN PX_NAV END) AS PX_NAV_2,
    MAX(CASE WHEN RN = 2 THEN LIBS END)   AS LIBS_2,

    MAX(CASE WHEN RN = 3 THEN PX_NAV END) AS PX_NAV_3,
    MAX(CASE WHEN RN = 3 THEN LIBS END)   AS LIBS_3,

    MAX(CASE WHEN RN = 4 THEN PX_NAV END) AS PX_NAV_4,
    MAX(CASE WHEN RN = 4 THEN LIBS END)   AS LIBS_4

FROM NUMBERED_DATA

GROUP BY
    CODE_PORTEFEUILLE,
    INST_CD,
    COTATION_DT,
    COURS,
    COURS_DTE,
    VALO_DT

ORDER BY INST_CD;
Hi,

Following additional checks on the Asset Price vs Bloomberg comparison, I identified an issue regarding the prices currently available in GP.

The Asset Price I retrieve comes directly from the inventory. The price I initially considered to be the Bloomberg price is actually the price resulting from the fund's Pricing Policy.

During my initial checks, I tested one or two positions and the prices happened to match Bloomberg, which led me to believe that this price could be used as the BBG reference.

However, after performing additional checks on more positions, I realized that this is not necessarily the case. The price depends on the Pricing Policy defined for the fund and therefore does not systematically represent a Bloomberg price.

As a result, we currently do not have a dedicated Bloomberg price available in GP that can be used for this reconciliation.

Retrieving Bloomberg prices directly and automatically would be a separate project.

In the meantime, you can still use my reconciliation to compare the Asset Price from the inventory against the Bloomberg price. However, the Bloomberg price file would need to be saved/exported so that my process can read it and perform the comparison.

The reconciliation would therefore compare:

- Asset Price: retrieved directly from the inventory
- BBG Price: retrieved from the saved Bloomberg price file

This would allow us to continue using the reconciliation while a more automated solution for retrieving Bloomberg prices is considered.

Best regards,
Ali
