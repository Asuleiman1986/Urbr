Map m = (Map) object;

double open = m.get("VAR_OPEN") == null ? 0 :
    ((Number) m.get("VAR_OPEN")).doubleValue();

double afr = m.get("VAR_AFR") == null ? 0 :
    ((Number) m.get("VAR_AFR")).doubleValue();

double initial = m.get("VAR_INITIAL") == null ? 0 :
    ((Number) m.get("VAR_INITIAL")).doubleValue();

// Arrondir à deux décimales
open = Math.round(open * 100.0) / 100.0;
afr = Math.round(afr * 100.0) / 100.0;
initial = Math.round(initial * 100.0) / 100.0;

// 1. OPEN <-> AFR
double match12 = 0;
if (open * afr < 0) {
    match12 = Math.min(Math.abs(open), Math.abs(afr));
    open += open < 0 ? match12 : -match12;
    afr += afr < 0 ? match12 : -match12;
}

// 2. OPEN <-> INITIAL
double match13 = 0;
if (open * initial < 0) {
    match13 = Math.min(Math.abs(open), Math.abs(initial));
    open += open < 0 ? match13 : -match13;
    initial += initial < 0 ? match13 : -match13;
}

// 3. AFR <-> INITIAL
double match23 = 0;
if (afr * initial < 0) {
    match23 = Math.min(Math.abs(afr), Math.abs(initial));
    afr += afr < 0 ? match23 : -match23;
    initial += initial < 0 ? match23 : -match23;
}

m.put("MATCH_1_2", match12);
m.put("MATCH_1_3", match13);
m.put("MATCH_2_3", match23);

// Soldes après matching
m.put("REMAIN_OPEN", open);
m.put("REMAIN_AFR", afr);
m.put("REMAIN_INITIAL", initial);

return m;
