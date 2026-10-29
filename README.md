Map m = (Map) object;

Object value = m.get("REMAIN_OPEN");

if (value == null) {
    throw new RuntimeException("REMAIN_OPEN est null ou absent");
}

double remain = ((Number) value).doubleValue();

// Arrondi à 2 décimales
remain = Math.round(remain * 100.0) / 100.0;

// Garder uniquement les montants non nuls
return Math.abs(remain) > 0;
