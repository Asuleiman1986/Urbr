Map m = (Map) object;

// 1. Récupération des données
String fund = m.get("FUND_CODE").toString();
String broker = m.get("BROKER").toString();
String currency = m.get("CURRENCY").toString();

String fromAccount = m.get("FROM_ACCOUNT").toString();
String toAccount = m.get("TO_ACCOUNT").toString();

double amount = ((Number) m.get("AMOUNT")).doubleValue();

// 2. Code provenant de Transco (colonne E)
// À adapter au nom réel de ton champ après le merge
String transcoCode = m.get("TRANSCO_CODE").toString();

// Compléter avec des zéros à gauche sur 7 caractères
String code7 = String.format("%7s", transcoCode)
                     .replace(' ', '0');

// 3. Date correspondant à la cellule Excel O1
// On suppose que le champ contient une date yyyy-MM-dd
String dateStr = m.get("DATE_D").toString().substring(0, 10);

java.text.SimpleDateFormat input =
    new java.text.SimpleDateFormat("yyyy-MM-dd");

java.util.Date date = input.parse(dateStr);

String dateSlash =
    new java.text.SimpleDateFormat("dd/MM/yyyy").format(date);

String dateCompact =
    new java.text.SimpleDateFormat("yyyyMMdd").format(date);

// 4. Montant arrondi à deux décimales
String amountStr = String.format(
    java.util.Locale.US, "%.2f", amount
);

// 5. Construction de la ligne CLEARER
String clearer =
    "CLEARER_" + broker + "_OD;"
    + code7 + ";"
    + dateSlash + ";"
    + dateSlash + ";"
    + "TDF;CR;"
    + fromAccount + ";"
    + toAccount + ";"
    + amountStr + ";"
    + currency + ";"
    + "MARGIN TRANSFER NAV_" + broker
    + " " + dateCompact;

// 6. Ajout du résultat
m.put("CLEARER_LINE", clearer);

return m;
