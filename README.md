Map m = (Map) object;

String currency = m.get("CURRENCY").toString();
String fund = m.get("FUND_CODE").toString();
String suffix = fund.substring(fund.length() - 2);

double open = ((Number) m.get("VAR_OPEN")).doubleValue();
double amount = ((Number) m.get("MATCH_1_2")).doubleValue();

String accountOpen = "MA1Q1" + currency + suffix;
String accountAfr = "CG1Q1" + currency + suffix;

if (open < 0) {
    m.put("FROM_ACCOUNT", accountOpen);
    m.put("TO_ACCOUNT", accountAfr);
} else {
    m.put("FROM_ACCOUNT", accountAfr);
    m.put("TO_ACCOUNT", accountOpen);
}

m.put("AMOUNT", amount);
m.put("STATUS", "Matched");

return m;
