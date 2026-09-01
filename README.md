 Stores the specific characteristics of
negotiable debt instruments referenced in the system. It complements the
general instrument information with debt-specific data such as issuer,
counterparty, maturity and issuance dates, nominal and redemption
amounts, interest-rate and spread information, capitalization and
interest periodicity, rating, settlement rules, taxation, amortization
and optional call or put features. Each instrument is identified by a
CODE_VALEUR, which can be used to link these characteristics to related
instrument and position data.

COLUMN DEFINITIONS

CLASSE Identifies the class to which the negotiable debt instrument
belongs according to the classification used in the source system.

CODE_CONTREPARTIE Identifies the counterparty associated with the
negotiable debt instrument, when applicable.

CODE_COURBE_TAUX Identifies the interest rate curve used as a reference
for pricing, interest calculation or valuation of the instrument.

CODE_EMETTEUR Identifies the issuer of the negotiable debt instrument.

CODE_RATING Identifies the credit rating assigned to the instrument or
its issuer according to the rating information maintained in the source
system.

CODE_VALEUR Unique internal identifier of the negotiable debt
instrument, used to link the instrument to related instrument and
position data.

COURBE_SPREAD Identifies the spread curve associated with the negotiable
debt instrument for pricing or valuation purposes.

COURBE_SPREAD_FLUX_FUTURS Identifies the spread curve used for the
projection or valuation of future cash flows.

COURBE_TAUX_FLUX_FUTURS Identifies the interest rate curve used for the
projection or valuation of future cash flows.

DATE_1ERE_CAPITALISATION Date from which the first capitalization period
of the instrument starts or is applied.

DATE_DERN_TOMBEE Date of the most recent contractual payment or maturity
event associated with the instrument.

DATE_ECHEANCE Date on which the negotiable debt instrument reaches its
contractual maturity.

DATE_ECHEANCE_PENSION Maturity date associated with the repo or pension
arrangement linked to the instrument, when applicable.

DATE_EMISSION Date on which the negotiable debt instrument was issued.

DATE_EMISSION_PENSION Issue or start date associated with the repo or
pension arrangement linked to the instrument, when applicable.

DATE_PREMIERE_COTATION Date on which the negotiable debt instrument was
first quoted or listed.

DATE_SAISIE Date on which the instrument information was entered or
recorded in the system.

DELTA_VALO_CRNE Indicates the valuation delta parameter associated with
the negotiable debt instrument. The exact business interpretation
depends on the valuation rules configured in the source system.

DEVISE_VALEUR Identifies the currency in which the negotiable debt
instrument is denominated.

EXPRESSION_COURS Specifies how the price or quotation of the negotiable
debt instrument is expressed.

FIN_DE_MOIS Indicates whether an end-of-month convention applies to the
instrument’s date calculations.

FLAG_CREANCE_CALL Indicates whether the negotiable debt instrument
includes a call feature allowing early redemption under defined
contractual conditions.

FLAG_CREANCE_PUT Indicates whether the negotiable debt instrument
includes a put feature allowing the holder to request early redemption
under defined contractual conditions.

HEURE_SAISIE Time at which the instrument information was entered or
recorded in the system.

ID_USERNAME Identifies the user or process that entered or last recorded
the instrument information in the system.

LIB_CREANCE_NEGO_C Short business description or label of the negotiable
debt instrument.

LIB_CREANCE_NEGO_L Long business description or label of the negotiable
debt instrument.

MARCHE_NEGOCIATION Identifies the market or trading environment in which
the negotiable debt instrument is negotiated.

MNEMONIQUE_VALEUR Mnemonic or abbreviated identifier used to represent
the negotiable debt instrument in the source system.

MONTANT_EMISSION Total amount issued for the negotiable debt instrument.

MONTANT_REMB Amount contractually payable upon redemption of the
negotiable debt instrument.

NOM_TAUX Identifies the interest rate or reference rate applicable to
the negotiable debt instrument.

NOM_TAUX_PENSION Identifies the interest rate or reference rate
applicable to the repo or pension arrangement associated with the
instrument.

NOMINAL_PENSION Nominal amount associated with the repo or pension
arrangement linked to the instrument.

NOMINAL_VALEUR Nominal or face value of the negotiable debt instrument.

PERIODE_CAPITALISATION Defines the capitalization period applicable to
the interest calculation of the instrument.

PERIODE_LINEARISATION Defines the period used for linearization or
accrual allocation in the calculation of the instrument.

PERIODICITE_INT Defines the frequency at which interest is calculated or
paid for the negotiable debt instrument.

PLACE_COTATION Identifies the exchange, market place or quotation venue
associated with the negotiable debt instrument.

PRIX_EMISSION Price at which the negotiable debt instrument was issued.

QUANTIEME_CALCUL Specifies the day of the relevant period used in
contractual interest or date calculations.

REGLE_CALCUL_AJUSTEMENT Identifies the adjustment rule applied to
contractual date or calculation conventions.

SPOT_LAG Defines the number of business days between the relevant
reference date and the applicable spot or settlement date.

SPREAD_TAUX Specifies the spread applied to the reference interest rate
of the negotiable debt instrument.

SPREAD_TAUX_FLUX_FUTURS Specifies the spread applied when projecting or
valuing future cash flows of the instrument.

SPREAD_TAUX_PENSION Specifies the interest rate spread applicable to the
repo or pension arrangement associated with the instrument.

SPREAD_TAUX_VALO Specifies the spread applied for valuation purposes.

TYPE_AMORTISSEMENT Identifies the amortization method or schedule
applicable to the negotiable debt instrument.

TYPE_CALENDRIER Identifies the calendar convention used to determine
applicable business days for the instrument.

TYPE_CREANCE Identifies the business type or category of the negotiable
debt instrument.

TYPE_DECALAGE Identifies the convention used to shift a contractual date
when an adjustment is required.

TYPE_DELAI_REGLEMENT Identifies the settlement delay convention
applicable to the negotiable debt instrument.

TYPE_FISCALITE_VALEUR Identifies the taxation treatment or tax
classification applicable to the negotiable debt instrument.

TYPE_PENSION Identifies the type of repo or pension arrangement
associated with the instrument, when applicable.

TYPE_VALEUR_TCN Identifies the specific type of negotiable debt security
(TCN) according to the classification used in the source system.

UNITE_PERIODICITE Identifies the unit used to express a periodicity or
frequency associated with the instrument.
