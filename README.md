  Stores the specific characteristics of foreign
exchange forward (FX Forward) instruments, including the currencies
exchanged, counterparty, nominal amount, spot and forward exchange
rates, maturity, fixing, settlement and trading information. Each
instrument is identified by a CODE_VALEUR, which can be used to link the
FX Forward characteristics to other instrument and position data.

COLUMN DEFINITIONS

CODE_CONTREPARTIE Identifies the counterparty with which the FX Forward
contract was entered into.

CODE_DEVISE_REGL Identifies the currency used for the settlement of the
FX Forward contract.

CODE_FIXING_REGL Identifies the fixing rule or fixing reference used for
the settlement of the FX Forward contract.

CODE_INTERM_NEGOCE_1 Identifies the intermediary involved in the
negotiation of the FX Forward contract.

CODE_VALEUR Unique internal identifier of the FX Forward instrument,
used to link the contract to related instrument and position data.

COMPENSATEUR Identifies the clearing entity or clearing agent associated
with the FX Forward contract, when applicable.

COURS_A_TERME Forward exchange rate agreed for the exchange of the two
currencies at the maturity of the FX Forward contract.

COURS_SPOT Spot exchange rate associated with the currency pair and used
as a reference for the FX Forward contract.

DATE_CREATION Date on which the FX Forward instrument or contract record
was created.

DATE_ECHEANCE Date on which the FX Forward contract reaches maturity and
the agreed currency exchange is due to be settled.

DATE_SAISIE Date on which the FX Forward information was entered or
recorded in the system.

DECALAGE_DATE_FIXING Defines the date adjustment or offset applied to
determine the fixing date of the FX Forward contract.

DELTA_VALO_CAT Indicates the valuation delta parameter associated with
the FX Forward instrument. The exact business interpretation depends on
the valuation rules configured in the source system.

DEVISE_COTATION Identifies the quotation currency used to express the
exchange rate of the FX Forward currency pair.

DEVISE_COTEE Identifies the quoted currency of the FX Forward currency
pair.

HEURE_SAISIE Time at which the FX Forward information was entered or
recorded in the system.

ID_USERNAME Identifies the user or process that entered or last recorded
the FX Forward information in the system.

INDICATEUR_COMPENSATION Indicates whether the FX Forward contract is
subject to a clearing process.

LIB_CHANGE_A_TERME_C Short business description or label of the FX
Forward instrument.

LIB_CHANGE_A_TERME_L Long business description or label of the FX
Forward instrument.

MARCHE_NEGOCIATION Identifies the market or trading environment in which
the FX Forward contract is negotiated.

NATURE_CONTRAT Identifies the business nature or contractual type of the
FX Forward transaction.

NOMINAL Nominal amount of the FX Forward contract representing the
contractual amount to be exchanged.

PLACE_COTATION Identifies the quotation or market place associated with
the FX Forward instrument.

REMUNERATION_COTATION Specifies the remuneration or interest-related
information associated with the quotation currency of the FX Forward
contract.

REMUNERATION_COTEE Specifies the remuneration or interest-related
information associated with the quoted currency of the FX Forward
contract.

SENS_OPERATION Indicates the direction of the FX Forward transaction,
defining the buy/sell orientation of the               
        COUNTERPARTY
    </div>

    <div
        id="${tabID}-CTPYButtons"
        style="max-height:120px; overflow:auto;">
    </div>

    <div
        id="${tabID}-QuantitySummary"
        style="
            margin-top:10px;
            border-top:1px solid #333;
            background:#161616;
        ">
    </div>
</div>
Ton bloc de gauche devient donc :
HTML
3. Appelle la fonction au chargement initial
Après :
renderCTPYButtons(view, tabID, table, view._CTPYList);
renderStatusDonut(view, tabID, table.getData("active"), "ALL");
ajoute :
renderQuantitySummary(tabID, table.getData("active"));
4. Mets à jour les sommes lors des filtres
Dans :
table.on("dataFiltered", function(filters, rows) {
ajoute après renderStatusDonut(...) :
renderQuantitySummary(tabID, activeData);
Le bloc complet devient :
table.on("dataFiltered", function(filters, rows) {
    const activeData = rows.map(r => r.getData());

    const suffix =
        (view._selectedCTPY && view._selectedCTPY !== "ALL")
            ? ("CTPY: " + view._selectedCTPY)
            : "FILTERED";

    renderStatusDonut(view, tabID, activeData, suffix);
    renderQuantitySummary(tabID, activeData);
});
5. Mets aussi à jour après modification du tableau
Dans :
table.on("dataChanged", function() {
ajoute :
renderQuantitySummary(tabID, table.getData("active"));
Donc :
table.on("dataChanged", function() {
    const activeData = table.getData("active");

    renderStatusDonut(
        view,
        tabID,
        activeData,
        (view._selectedCTPY && view._selectedCTPY !== "ALL")
            ? ("CTPY: " + view._selectedCTPY)
            : "ALL"
    );

    renderQuantitySummary(tabID, activeData);
});






1. Ajoute cette fonction après renderStatusDonut
function renderQuantitySummary(tabID, rows) {
    let sumQuantity = 0;
    let sumQuantityGP = 0;

    (rows || []).forEach(r => {
        sumQuantity += Number(r.QUANTITY) || 0;
        sumQuantityGP += Number(r.QUANTITY_GP) || 0;
    });

    const diff = sumQuantityGP - sumQuantity;

    const el = document.getElementById(tabID + "-QuantitySummary");
    if (!el) return;

    el.innerHTML = `
        <div style="
            display:flex;
            justify-content:space-between;
            align-items:center;
            gap:20px;
            padding:10px 12px;
            font-size:14px;
        ">
            <div style="flex:1;">
                <div style="color:rgba(209,232,238,0.70); font-size:12px;">
                    QUANTITY
                </div>
                <div style="font-weight:700; color:#fff;">
                    ${sumQuantity.toLocaleString()}
                </div>
            </div>

            <div style="flex:1;">
                <div style="color:rgba(209,232,238,0.70); font-size:12px;">
                    QUANTITY GP
                </div>
                <div style="font-weight:700; color:#fff;">
                    ${sumQuantityGP.toLocaleString()}
                </div>
            </div>

            <div style="flex:1;">
                <div style="color:rgba(209,232,238,0.70); font-size:12px;">
                    DIFFERENCE
                </div>
                <div style="
                    font-weight:700;
                    color:${diff === 0 ? "#71886f" : "#f4d4df"};
                ">
                    ${diff.toLocaleString()}
                </div>
            </div>
        </div>
    `;
}
2. Dans ton root.innerHTML
Dans le bloc COUNTERPARTY, juste après :
HTML
<div id="${tabID}-CTPYButtons" style="max-height:120px; overflow:auto;"></div>
ajoute :
HTML
<div
    id="${tabID}-QuantitySummary"
    style="
        margin-top:10px;
        border-top:1px solid #333;
        background:#161616;
    ">
</div>
<div style="width:520px; border:1px solid #333; padding:10px; background:#161616; box-sizing:border-box;">
    <div style="
        color:rgba(209,232,238,0.85);
        font-weight:600;
        margin-bottom:8px;
        text-transform:uppercase;
    ">
        COUNTERPARTY
    </div>

    <div
        id="${tabID}-CTPYButtons"
        style="max-height:120px; overflow:auto;">
    </div>

    <div
        id="${tabID}-QuantitySummary"
        style="
            margin-top:10px;
            border-top:1px solid #333;
            background:#161616;
        ">
    </div>
</div>
Ton bloc de gauche devient donc :
HTML





Voici une base propre et extensible en Java.
import java.time.DayOfWeek;
import java.time.LocalDate;
import java.time.Month;
import java.time.temporal.TemporalAdjusters;
import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;

public class HolidayCalculator {

    public record Holiday(String name, LocalDate date) {
    }

    public static List<Holiday> getFrenchHolidays(int year) {
        List<Holiday> holidays = new ArrayList<>();

        /*
         * Jours fériés à date fixe
         */
        holidays.add(new Holiday(
                "Jour de l'An",
                LocalDate.of(year, Month.JANUARY, 1)
        ));

        holidays.add(new Holiday(
                "Fête du Travail",
                LocalDate.of(year, Month.MAY, 1)
        ));

        holidays.add(new Holiday(
                "Victoire 1945",
                LocalDate.of(year, Month.MAY, 8)
        ));

        holidays.add(new Holiday(
                "Fête nationale",
                LocalDate.of(year, Month.JULY, 14)
        ));

        holidays.add(new Holiday(
                "Assomption",
                LocalDate.of(year, Month.AUGUST, 15)
        ));

        holidays.add(new Holiday(
                "Toussaint",
                LocalDate.of(year, Month.NOVEMBER, 1)
        ));

        holidays.add(new Holiday(
                "Armistice",
                LocalDate.of(year, Month.NOVEMBER, 11)
        ));

        holidays.add(new Holiday(
                "Noël",
                LocalDate.of(year, Month.DECEMBER, 25)
        ));

        /*
         * Calcul du dimanche de Pâques
         */
        LocalDate easterSunday = calculateEasterSunday(year);

        /*
         * Jours fériés dépendant de Pâques
         */
        holidays.add(new Holiday(
                "Lundi de Pâques",
                easterSunday.plusDays(1)
        ));

        holidays.add(new Holiday(
                "Ascension",
                easterSunday.plusDays(39)
        ));

        holidays.add(new Holiday(
                "Lundi de Pentecôte",
                easterSunday.plusDays(50)
        ));

        holidays.sort(Comparator.comparing(Holiday::date));

        return holidays;
    }

    /**
     * Calcule le dimanche de Pâques selon l'algorithme
     * de Meeus/Jones/Butcher.
     */
    public static LocalDate calculateEasterSunday(int year) {
        if (year < 1583) {
            throw new IllegalArgumentException(
                    "Cette méthode est prévue pour le calendrier grégorien à partir de 1583."
            );
        }

        int a = year % 19;
        int b = year / 100;
        int c = year % 100;
        int d = b / 4;
        int e = b % 4;
        int f = (b + 8) / 25;
        int g = (b - f + 1) / 3;
        int h = (19 * a + b - d - g + 15) % 30;
        int i = c / 4;
        int k = c % 4;
        int l = (32 + 2 * e + 2 * i - h - k) % 7;
        int m = (a + 11 * h + 22 * l) / 451;

        int month = (h + l - 7 * m + 114) / 31;
        int day = ((h + l - 7 * m + 114) % 31) + 1;

        return LocalDate.of(year, month, day);
    }

    /**
     * Calcule, par exemple, le quatrième jeudi de novembre.
     */
    public static LocalDate getNthWeekdayOfMonth(
            int year,
            Month month,
            DayOfWeek dayOfWeek,
            int occurrence
    ) {
        if (occurrence < 1 || occurrence > 5) {
            throw new IllegalArgumentException(
                    "L'occurrence doit être comprise entre 1 et 5."
            );
        }

        LocalDate firstDayOfMonth = LocalDate.of(year, month, 1);

        LocalDate result = firstDayOfMonth.with(
                TemporalAdjusters.dayOfWeekInMonth(occurrence, dayOfWeek)
        );

        if (result.getMonth() != month) {
            throw new IllegalArgumentException(
                    "Cette occurrence n'existe pas dans le mois demandé."
            );
        }

        return result;
    }

    /**
     * Calcule, par exemple, le dernier lundi de mai.
     */
    public static LocalDate getLastWeekdayOfMonth(
            int year,
            Month month,
            DayOfWeek dayOfWeek
    ) {
        return LocalDate.of(year, month, 1)
                .with(TemporalAdjusters.lastInMonth(dayOfWeek));
    }

    public static void main(String[] args) {
        int startYear = 2026;
        int numberOfYears = 5;

        for (int year = startYear; year < startYear + numberOfYears; year++) {
            System.out.println();
            System.out.println("Jours fériés pour l'année " + year);
            System.out.println("--------------------------------");

            for (Holiday holiday : getFrenchHolidays(year)) {
                System.out.printf(
                        "%-25s : %s%n",
                        holiday.name(),
                        holiday.date()
                );
            }
        }

        LocalDate example = getNthWeekdayOfMonth(
                2026,
                Month.NOVEMBER,
                DayOfWeek.THURSDAY,
                4
        );

        System.out.println();
        System.out.println("Quatrième jeudi de novembre 2026 : " + example);
    }
}
Principe pour le lundi de Pentecôte
Le programme commence par déterminer le dimanche de Pâques :
LocalDate easterSunday = calculateEasterSunday(year);
Puis il ajoute 50 jours :
LocalDate pentecostMonday = easterSunday.plusDays(50);
De la même manière :
LocalDate easterMonday = easterSunday.plusDays(1);
LocalDate ascension = easterSunday.plusDays(39);
LocalDate pentecostMonday = easterSunday.plusDays(50);
Pour rendre les règles modifiables
À terme, je te conseille de ne pas coder toutes les fêtes directement dans la classe. Tu pourrais enregistrer leurs règles dans un fichier JSON ou une base de données :
[
  {
    "name": "Noël",
    "ruleType": "FIXED_DATE",
    "month": 12,
    "day": 25
  },
  {
    "name": "Lundi de Pentecôte",
    "ruleType": "EASTER_OFFSET",
    "offsetDays": 50
  },
  {
    "name": "Fête spéciale",
    "ruleType": "NTH_WEEKDAY",
    "month": 11,
    "dayOfWeek": "THURSDAY",
    "occurrence": 4
  }
]
Le programme pourrait alors interpréter plusieurs types de règles :
public enum HolidayRuleType {
    FIXED_DATE,
    EASTER_OFFSET,
    NTH_WEEKDAY,
    LAST_WEEKDAY,
    MANUAL_DATE
}
Cette solution permettrait de gérer plusieurs pays sans modifier le code principal : chaque pays aurait simplement sa propre liste de règles.
 
 
 
 
 
 
 
 
 
 import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class IcsReader {

    public static void lireFichierICS(String cheminFichier) {

        try (BufferedReader reader = new BufferedReader(new FileReader(cheminFichier))) {

            String ligne;
            String summary = "";
            String date = "";

            while ((ligne = reader.readLine()) != null) {

                // Nom de l'événement
                if (ligne.startsWith("SUMMARY:")) {
                    summary = ligne.substring("SUMMARY:".length());
                }

                // Date de début
                if (ligne.startsWith("DTSTART")) {
                    date = ligne.substring(ligne.indexOf(":") + 1);
                }

                // Fin de l'événement : on affiche les informations
                if (ligne.equals("END:VEVENT")) {
                    System.out.println("--------------------------------");
                    System.out.println("Nom  : " + summary);
                    System.out.println("Date : " + date);
                }
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {

        String chemin = "C:\\Users\\ASULEIMAN\\Downloads\\Monaco_2027.ics";

        lireFichierICS(chemin);
    }
}
