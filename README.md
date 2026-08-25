Stores the specific characteristics of futures
contracts referenced in the system. It complements the general
instrument information with futures-specific data such as the underlying
instrument, contract identifier, maturity information, quotation and
settlement currencies, trading market, quotation place, conversion
factor, settlement method and applicable calendars. Each futures
contract is identified by a CODE_VALEUR, which can be used to link these
characteristics to related instrument and position data.

COLUMN DEFINITIONS

CATEGORIE_VALEUR_SOUS_JACENTE Identifies the financial instrument
category of the underlying asset on which the futures contract is based.

CODE_CONTREPARTIE Identifies the counterparty associated with the
futures contract, when applicable.

CODE_EMETTEUR Identifies the issuer or entity associated with the
futures contract.

CODE_VALEUR Unique internal identifier of the futures contract, used to
link the contract to related instrument and position data.

CODE_VALEUR_SOUS_JACENTE Identifies the underlying instrument on which
the futures contract is based.

DATE_ECHEANCE_PREVI Expected maturity or expiration date of the futures
contract.

DATE_SAISIE Date on which the futures contract information was entered
or recorded in the system.

DEVISE_CONTREPARTIE Identifies the currency associated with the
counterparty or opposite side of the futures contract.

DEVISE_COTATION Identifies the currency in which the futures contract is
quoted.

ECHELON_COTATION Defines the minimum quotation increment or price step
applicable to the futures contract.

FACTEUR_DE_CONVERSION Conversion factor applied to the futures contract
or its underlying instrument for valuation, quotation or settlement
purposes.

HEURE_SAISIE Time at which the futures contract information was entered
or recorded in the system.

ID_USERNAME Identifies the user or process that entered or last recorded
the futures contract information in the system.

IDENTIFIANT_CONTRAT_FUTURE Identifies the specific futures contract
according to the contract identification used in the source system.

JOUR_ECHEANCE Specifies the day used to determine the expiration or
maturity of the futures contract.

LIB_CONTRAT_FUTURE_C Short business description or label of the futures
contract.

LIB_FUTURE_C Short business description or label of the futures
instrument.

LIB_FUTURE_L Long business description or label of the futures
instrument.

MARCHE_NEGOCIATION Identifies the market or trading environment in which
the futures contract is negotiated.

MODE_REGLEMENT Specifies the settlement method applicable to the futures
contract, such as cash settlement or physical delivery, where
applicable.

MOIS_ECHEANCE Specifies the month used to determine the expiration or
maturity of the futures contract.

NATURE_CONTRAT Identifies the business nature or contractual type of the
futures contract.

PLACE_COTATION Identifies the exchange or quotation place on which the
futures contract is listed or quoted.

TYPE_CALENDRIER Identifies the calendar convention used to determine
applicable business days for the futures contract.

TYPE_CALENDRIER_FERIE Identifies the holiday calendar used when
determining business days, maturity or settlement dates for the futures
contract.
              
              
              
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
