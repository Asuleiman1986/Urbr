DESCRIPTIF_REMERE

TABLE DEFINITION
Stores the specific characteristics of repurchase or repurchase-option (réméré) contracts referenced in the system. It provides information about the buyer and seller, related portfolios, committed and settlement amounts, contractual dates, interest and indexation conditions, reimbursement terms, late-interest conditions, valuation parameters and settlement rules. Each contract is identified by a CODE_VALEUR, which can be used to link these characteristics to related instrument and position data.

COLUMN DEFINITIONS

BASE_CALCUL
Defines the calculation basis used to determine interest or other contractual amounts.

CODE_ACHETEUR
Identifies the buyer associated with the contract.

CODE_COURBE_TAUX
Identifies the interest-rate curve used for calculation or valuation purposes.

CODE_DEVISE_FIXING
Identifies the currency used for the fixing or reference-rate determination.

CODE_EVENEMENT_ENGAGEMENT
Identifies the event or event type associated with the contractual commitment.

CODE_PORTEF_ACHAT
Identifies the portfolio associated with the buying side of the contract.

CODE_PORTEF_VENDEUR
Identifies the portfolio associated with the selling side of the contract.

CODE_VALEUR
Unique identifier of the réméré contract or instrument in the system.

CODE_VENDEUR
Identifies the seller associated with the contract.

COMPENSATEUR
Identifies the clearing member, clearing agent or entity involved in the clearing process, when applicable.

COURBE_SPREAD
Identifies the spread curve used for calculation or valuation purposes.

COURBE_SPREAD_FLUX_FUTURS
Identifies the spread curve used to value or project future contractual cash flows.

COURBE_TAUX_FLUX_FUTURS
Identifies the interest-rate curve used to value or project future contractual cash flows.

DATE_1ERE_CAPITALISATION
Date on which the first capitalization of interest occurs.

DATE_DENOUEMENT
Date on which the contract is settled or unwound and the related contractual obligations are completed.

DATE_DERN_TOMBEE
Date of the most recent or final scheduled contractual payment.

DATE_ECHEANCE_PREVI
Expected or forecast maturity date of the contract.

DATE_ENGAGEMENT
Date on which the contractual commitment becomes effective.

DATE_INTERET_RETARD
Date from which late-payment interest starts to accrue.

DATE_PREMIERE_ECHEANCE
Date of the first scheduled contractual payment or maturity.

DATE_SAISIE
Date on which the record was entered or updated in the source system.

DELAI_INTERET_RETARD
Defines the delay or grace period before late-payment interest becomes applicable.

DELTA_VALO_REME
Valuation adjustment or parameter used by the source system for the valuation of the réméré contract.

DEVISE_VALEUR
Currency in which the contract or instrument is denominated.

FIN_DE_MOIS
Indicates whether an end-of-month convention applies to contractual date calculations.

FLAG_REMBOURSEMENT
Indicates whether reimbursement conditions or a reimbursement mechanism apply to the contract.

FLAG_TRIPARTITES
Indicates whether the contract involves a triparty arrangement with a third party providing operational or collateral-related services.

HEURE_SAISIE
Time at which the record was entered or updated in the source system.

ID_USERNAME
Identifier of the user or process that created or updated the record.

INDICATEUR_COMPENSATION
Indicates whether the contract is subject to clearing or compensation.

LIB_VALEUR_C
Short business label or description of the réméré contract or instrument.

MODE_CALCUL_FRAIS
Specifies the calculation method used to determine fees associated with the contract.

MONTANT_DENOUEMENT
Amount payable or exchanged when the contract is settled or unwound.

MONTANT_ENGAGEMENT
Amount associated with the initial contractual commitment.

MONTANT_INDEMNITE_PREVI
Expected or forecast indemnity amount associated with the contract, when applicable.

MONTANT_INTERET_RETARD
Amount of late-payment interest associated with delayed settlement or payment.

MONTANT_REMB
Contractual reimbursement amount associated with the contract.

NATURE_INDEXATION
Identifies the nature or basis of the indexation applied to contractual amounts or interest.

NOM_TAUX
Identifies the reference interest rate applicable to the contract.

OBJECTIF_CONTRAT
Identifies the business, investment or financing purpose associated with the contract.

PERIODE_CAPITALISATION
Defines the period over which interest capitalization is applied.

PERIODICITE_INT
Defines the frequency of interest calculations or payments.

PLACE_COTATION
Identifies the quotation or reference place associated with the contract or instrument.

REGLE_CALCUL_AJUSTEMENT
Specifies the business rule used to adjust contractual dates or calculated amounts.

SPOT_LAG
Number of business days used as the standard delay between the transaction date and the effective settlement date.

SPREAD_TAUX
Spread applied to the reference interest rate of the contract.

SPREAD_TAUX_FLUX_FUTURS
Spread applied when projecting or valuing future contractual cash flows.

SPREAD_TAUX_VALO
Spread used specifically for valuation purposes.

STATUT_LIGNE_REMERE
Identifies the current status of the réméré contract record or line.

TYPE_AMORTISSEMENT
Specifies the amortization method applicable to the contract, when relevant.

TYPE_CALENDRIER
Identifies the business calendar used for contractual date calculations and adjustments.

TYPE_DENOUEMENT
Identifies the method or type of settlement used to unwind or complete the contract.

TYPE_FISCALITE_VALEUR
Identifies the taxation treatment or fiscal classification applicable to the instrument or contract.

TYPE_TRANSACTION_ENGAGEMENT
Identifies the type of transaction associated with the contractual commitment.


DESCRIPTIF_OPTION

TABLE DEFINITION
Stores the specific characteristics of option instruments referenced in the system. It provides information about the underlying instrument, option type and direction, strike price, maturity, quotation and settlement conventions, currencies, contract identifiers, trading characteristics, margin requirements, interest and premium periodicity, valuation parameters, calendars and other option-specific terms. Each option is identified by a CODE_VALEUR, which can be used to link these characteristics to related instrument and position data.

COLUMN DEFINITIONS

CATEGORIE_VALEUR_SOUS_JACENTE
Identifies the asset category of the underlying instrument on which the option is based.

CLASSE
Identifies the classification or business class assigned to the option.

CLASSEMENT_VALEUR
Defines the classification or ranking assigned to the option within the source system.

CODE_CONTREPARTIE
Identifies the counterparty associated with the option contract.

CODE_EMETTEUR
Identifies the issuer associated with the option or its contractual reference.

CODE_VALEUR
Unique identifier of the option instrument in the system.

CODE_VALEUR_SOUS_JACENTE
Identifies the underlying instrument or asset on which the option is based.

COMPENSATEUR
Identifies the clearing member, clearing agent or entity involved in the clearing of the option, when applicable.

DATE_DEPART_INTERETS
Date from which interest related to the option contract starts to accrue, when applicable.

DATE_ECHEANCE_PREVI
Expected or forecast maturity or expiry date of the option.

DATE_SAISIE
Date on which the record was entered or updated in the source system.

DELTA_VALO_OPTI
Valuation adjustment or parameter used by the source system for the valuation of the option.

DEVISE_CONTREPARTIE
Identifies the currency associated with the counterparty side of the option contract.

DEVISE_COTATION
Identifies the currency in which the option is quoted.

ECHELON_COTATION
Defines the quotation increment or price step used for the option.

EXPRESSION_COURS
Specifies how the option price or quotation is expressed.

EXPRESSION_QUANTITE
Specifies how the quantity or contract amount of the option is expressed.

FACTEUR_DE_CONVERSION
Conversion factor applied when converting contract quantities, prices or values.

FIN_DE_MOIS
Indicates whether an end-of-month convention applies to contractual date calculations.

FLAG_MARGE
Indicates whether margin requirements apply to the option contract.

HEURE_SAISIE
Time at which the record was entered or updated in the source system.

ID_USERNAME
Identifier of the user or process that created or updated the record.

IDENTIFIANT_CONTRAT_OPTION
Identifies the standardized or internal option contract definition associated with the instrument.

INDICATEUR_COMPENSATION
Indicates whether the option is subject to clearing or compensation.

JOUR_ECHEANCE
Identifies the contractual day used to determine the option expiry or maturity date.

LIB_CONTRAT_OPTION_C
Short business label or description of the option contract definition.

LIB_OPTION_C
Short business label or description of the option instrument.

LIB_OPTION_L
Long business label or detailed description of the option instrument.

MARCHE_NEGOCIATION
Identifies the market or trading environment in which the option is negotiated.

MODE_REGLEMENT
Specifies the settlement method applicable to the option, such as cash or physical settlement.

MOIS_ECHEANCE
Identifies the contractual month used to determine the option expiry or maturity.

NATURE_CONTRAT
Identifies the contractual nature or business type of the option contract.

PERIODICITE_INT
Defines the frequency of interest calculations or payments associated with the option, when applicable.

PERIODICITE_PRIME
Defines the frequency at which the option premium is calculated or paid, when applicable.

PLACE_COTATION
Identifies the quotation or trading place associated with the option.

PRIX_EXERCICE
Strike price at which the option holder may buy or sell the underlying instrument according to the option terms.

QUANTIEME_CALCUL
Specifies the date or day-related calculation parameter used for contractual calculations.

QUOTITE_NEGO
Defines the standard trading lot or minimum negotiable quantity of the option.

SENS_COTATION
Indicates the quotation direction or convention used to express the option price or related exchange rate.

SENS_OPTION
Indicates the contractual direction of the option, such as the right to buy or the right to sell the underlying instrument.

SOUS_CATEGORIE_VALEUR
Identifies the business subcategory of the option instrument.

SPOT_LAG
Number of business days used as the standard delay between the transaction date and the effective settlement date.

TAUX_PLAFOND
Upper interest-rate limit or cap applicable to the option structure, when relevant.

TAUX_PLANCHER
Lower interest-rate limit or floor applicable to the option structure, when relevant.

TYPE_CALENDRIER
Identifies the business calendar used for option date calculations and adjustments.

TYPE_CALENDRIER_FERIE
Identifies the holiday calendar used to determine non-business days for the option.

TYPE_OBJECTIF
Identifies the general business or investment objective associated with the option.

TYPE_OBJECTIF_OPTION
Identifies the option-specific objective or purpose defined in the source system.

TYPE_OPTION
Identifies the type of option according to the contractual option characteristics defined by the source system.

VARIATION_MINI
Defines the minimum permitted price variation or tick size for the option.




DESCRIPTIF_CONTRAT_OPTION

TABLE DEFINITION
Stores the standardized characteristics and reference parameters of option contract definitions used in the system. It provides contract-level information such as the underlying asset category and identifier, counterparty and issuer, quotation and settlement currencies, trading conventions, contract size and conversion parameters, margin requirements, settlement method, option type, calendars and other characteristics shared by options linked to the same contract definition. The IDENTIFIANT_CONTRAT_OPTION can be used to associate these contract characteristics with related option instruments.

COLUMN DEFINITIONS

CATEGORIE_VALEUR_SOUS_JACENTE
Identifies the asset category of the underlying instrument on which the option contract is based.

CLASSE
Identifies the classification or business class assigned to the option contract.

CLASSEMENT_VALEUR
Defines the classification or ranking assigned to the option contract within the source system.

CODE_CONTREPARTIE
Identifies the counterparty associated with the option contract.

CODE_EMETTEUR
Identifies the issuer associated with the option contract or its contractual reference.

CODE_VALEUR_SOUS_JACENTE
Identifies the underlying instrument or asset on which the option contract is based.

DATE_SAISIE
Date on which the contract definition was entered or updated in the source system.

DELTA_VALO_OPTI
Valuation adjustment or parameter used by the source system for option valuation.

DEVISE_CONTREPARTIE
Identifies the currency associated with the counterparty side of the option contract.

DEVISE_COTATION
Identifies the currency in which the option contract is quoted.

ECHELON_COTATION
Defines the quotation increment or price step used for the option contract.

EXPRESSION_COURS
Specifies how the option contract price or quotation is expressed.

EXPRESSION_QUANTITE
Specifies how the quantity or contract amount is expressed.

FACTEUR_DE_CONVERSION
Conversion factor applied when converting contract quantities, prices or values.

FIN_DE_MOIS
Indicates whether an end-of-month convention applies to contractual date calculations.

FLAG_MARGE
Indicates whether margin requirements apply to the option contract.

HEURE_SAISIE
Time at which the contract definition was entered or updated in the source system.

ID_USERNAME
Identifier of the user or process that created or updated the contract definition.

IDENTIFIANT_CONTRAT_OPTION
Unique or business identifier of the standardized option contract definition in the system.

LIB_CONTRAT_OPTION_C
Short business label or description of the option contract definition.

MARCHE_NEGOCIATION
Identifies the market or trading environment in which the option contract is negotiated.

MODE_REGLEMENT
Specifies the settlement method applicable to the option contract, such as cash or physical settlement.

NATURE_CONTRAT
Identifies the contractual nature or business type of the option contract.

PERIODICITE_INT
Defines the frequency of interest calculations or payments associated with the option contract, when applicable.

PLACE_COTATION
Identifies the quotation or trading place associated with the option contract.

QUANTIEME_CALCUL
Specifies the date or day-related calculation parameter used for contractual calculations.

QUOTITE_NEGO
Defines the standard trading lot or minimum negotiable quantity of the option contract.

SENS_COTATION
Indicates the quotation direction or convention used to express the option contract price or related exchange rate.

SOUS_CATEGORIE_VALEUR
Identifies the business subcategory assigned to the option contract.

SPOT_LAG
Number of business days used as the standard delay between the transaction date and the effective settlement date.

TYPE_CALENDRIER
Identifies the business calendar used for contract date calculations and adjustments.

TYPE_CALENDRIER_FERIE
Identifies the holiday calendar used to determine non-business days for the option contract.

TYPE_OBJECTIF
Identifies the general business or investment objective associated with the option contract.

TYPE_OPTION
Identifies the type of option according to the contractual option characteristics defined by the source system.

VARIATION_MINI
Defines the minimum permitted price variation or tick size for the option contract.
