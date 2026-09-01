
TABLE DEFINITION
Stores the specific characteristics of credit derivative instruments referenced in the system. It provides information about the contract, reference entity, issuer, counterparty, notional amount, interest and payment schedule, maturity, settlement terms, credit seniority, materiality thresholds, trading information and valuation parameters. Each credit derivative is identified by a CODE_VALEUR, which can be used to link these characteristics to related instrument and position data.

COLUMN DEFINITIONS

CLASSE
Identifies the classification or business class assigned to the credit derivative.

CODE_CONTREPARTIE
Identifies the counterparty associated with the credit derivative contract.

CODE_EMETTEUR
Identifies the issuer associated with the credit derivative or referenced instrument.

CODE_ENTITE_REFERENCE
Identifies the reference entity whose credit risk is used to determine the contractual protection or payoff of the credit derivative.

CODE_VALEUR
Unique identifier of the credit derivative instrument in the system.

COMPENSATEUR
Identifies the clearing member, clearing agent or entity involved in the clearing of the contract, when applicable.

DATE_DEPART_INTERETS
Date from which interest or periodic credit derivative payments start to accrue.

DATE_ECHEANCE
Contractual due date associated with the credit derivative or one of its payment obligations.

DATE_EMISSION
Date on which the credit derivative instrument or contract was issued or created.

DATE_MATURITE
Final maturity date of the credit derivative contract.

DATE_PREMIERE_ECHEANCE
Date of the first scheduled payment or contractual payment period.

DATE_SAISIE
Date on which the record was entered or last recorded in the source system.

DELTA_VALO_DECR
Valuation adjustment or delta parameter used by the source system for the valuation of the credit derivative.

DEVISE_REMBOURSEMENT
Currency in which any contractual reimbursement or settlement amount is paid.

DEVISE_VALEUR
Currency in which the credit derivative is denominated.

FIN_DE_MOIS
Indicates whether end-of-month conventions apply when calculating contractual dates.

HEURE_SAISIE
Time at which the record was entered or updated in the source system.

ID_USERNAME
Identifier of the user or process that created or updated the record.

INDICATEUR_COMPENSATION
Indicates whether the credit derivative is subject to clearing or compensation.

LIB_CONTRAT_C
Short description or short label of the credit derivative contract.

LIB_CONTRAT_L
Long description or detailed label of the credit derivative contract.

MARCHE_NEGOCIATION
Identifies the market or trading environment in which the credit derivative is negotiated.

MODE_REGLEMENT
Specifies the contractual settlement method used for the credit derivative.

MONTANT_REMBOURSEMENT
Amount payable as reimbursement or settlement under the contract, when applicable.

NATURE_CONTRAT
Identifies the contractual nature or type of the credit derivative.

NOM_TAUX
Identifies the interest rate or reference rate applicable to the contract.

NOMINAL_VALEUR
Notional amount used as the reference amount for calculating payments, protection or settlement under the credit derivative.

OBJECTIF_CONTRAT
Identifies the business or investment purpose associated with the credit derivative contract.

PERIODICITE_ECHEANCE
Defines the frequency between scheduled payment or contractual due dates.

PLACE_COTATION
Identifies the quotation or trading place associated with the credit derivative.

QUANTIEME_CALCUL
Specifies the day-count or date-related calculation parameter used for periodic calculations.

RANG_DERNIER_EMETTEUR
Identifies the last issuer rank considered when the credit derivative references a range or basket of issuers.

RANG_PREMIER_EMETTEUR
Identifies the first issuer rank considered when the credit derivative references a range or basket of issuers.

REGLE_CALCUL_AJUSTEMENT
Specifies the business rule used to adjust calculated contractual dates or amounts.

SOUS_CATEGORIE_VALEUR
Identifies the subcategory of the credit derivative instrument, such as a specific type of credit protection or credit-linked contract.

SPOT_LAG
Number of business days used as the standard settlement delay between the trade date and the effective settlement date.

SPREAD_TAUX
Spread applied to the relevant reference rate or used to represent the contractual credit spread of the derivative.

TAUX_REMBOURSEMENT
Rate used to determine the reimbursement or settlement amount under the contract.

TYPE_CALCUL_COUPON_REF
Specifies the calculation method used for the reference coupon or periodic reference payment.

TYPE_CALCUL_REMBOURSEMENT
Specifies the calculation method used to determine the reimbursement or settlement amount.

TYPE_CALENDRIER
Identifies the business calendar used for date calculations, payment schedules and adjustments.

TYPE_SEGNIORITE_EMETTEUR
Identifies the issuer seniority classification used by the source system for the referenced credit obligation.

TYPE_SENIORITE_EMETTEUR
Identifies the seniority level of the issuer's referenced debt or obligation, such as senior or subordinated debt.

TYPE_SEUIL_MATERIALITE
Identifies the type of materiality threshold used when assessing whether a credit-related event or amount is significant under the contract.

UNITE_PERIODICITE
Defines the unit used with the periodicity value, such as days, months or years.
