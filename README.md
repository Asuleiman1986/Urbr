Map m = (Map) object;

String currency = m.get("CURRENCY").toString();
String fund = m.get("FUND_CODE").toString();
String suffix = fund.substring(fund.length() - 2);

double remain = ((Number) m.get("REMAIN_OPEN")).doubleValue();
remain = Math.round(remain * 100.0) / 100.0;

String account = "MA1Q1" + currency + suffix;

m.put("FROM_ACCOUNT", remain < 0 ? account : "");
m.put("TO_ACCOUNT", remain > 0 ? account : "");
m.put("AMOUNT", Math.abs(remain));
m.put("STATUS", "Unmatched");

return m;



Map m = (Map) object;

String currency = m.get("CURRENCY").toString();
String fund = m.get("FUND_CODE").toString();
String suffix = fund.substring(fund.length() - 2);

double remain = ((Number) m.get("REMAIN_AFR")).doubleValue();
remain = Math.round(remain * 100.0) / 100.0;

String account = "CG1Q1" + currency + suffix;

m.put("FROM_ACCOUNT", remain < 0 ? account : "");
m.put("TO_ACCOUNT", remain > 0 ? account : "");
m.put("AMOUNT", Math.abs(remain));
m.put("STATUS", "Unmatched");

return m;




Map m = (Map) object;

String currency = m.get("CURRENCY").toString();
String fund = m.get("FUND_CODE").toString();
String suffix = fund.substring(fund.length() - 2);

double remain = ((Number) m.get("REMAIN_INITIAL")).doubleValue();
remain = Math.round(remain * 100.0) / 100.0;

String account = "DG1Q1" + currency + suffix;

m.put("FROM_ACCOUNT", remain < 0 ? account : "");
m.put("TO_ACCOUNT", remain > 0 ? account : "");
m.put("AMOUNT", Math.abs(remain));
m.put("STATUS", "Unmatched");

return m;




