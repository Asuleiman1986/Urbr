String dateStr = m.get("COB_DATE").toString().trim();

java.text.SimpleDateFormat sdf =
    new java.text.SimpleDateFormat(
        "MMMM d, yyyy HH:mm:ss",
        java.util.Locale.ENGLISH
    );

java.util.Date date = sdf.parse(dateStr);

m.put("COB_DATE", date);

return m;
