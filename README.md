Subject: RE: Mediolannum – Data Use Case – Prioritization

Hi Simon,

Just to clarify the structure of the file:

Price_N (column N) and Price OLIS (column U) are the same price. Price_N comes from GP, while Price OLIS is retrieved directly from OLIS. I included both only to confirm that the price stored in GP matches the one available in OLIS.

The additional prices available in OLIS are shown in Price_N_A, Price_N_B and Price_N_C, with their respective contributors in the adjacent columns.

If several contributors are listed for one price, it means that they all provide exactly the same price, so I grouped them together.

However, I have just noticed that for quite a lot of positions, we do not have any additional contributors available in OLIS. So this may not be the best solution for the reconciliation after all. These additional prices can still be useful for the analysis where available, but I agree that we need to find another reliable source of prices.

If the KL team has access to the Bloomberg prices, we could potentially use their data as the source for the reconciliation.

Best regards,
Ali
