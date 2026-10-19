<stream type="refact">
    <java>
        <![CDATA[
            Map m = (Map) object;

            double open = m.get("VAR_OPEN") == null
                ? 0.0
                : ((Number) m.get("VAR_OPEN")).doubleValue();

            double afr = m.get("VAR_AFR") == null
                ? 0.0
                : ((Number) m.get("VAR_AFR")).doubleValue();

            // Arrondir à 2 décimales
            open = Math.round(open * 100.0) / 100.0;
            afr = Math.round(afr * 100.0) / 100.0;

            double match = 0.0;

            // Matching si signes opposés
            if ((open < 0 && afr > 0) ||
                (open > 0 && afr < 0)) {

                match = Math.min(
                    Math.abs(open),
                    Math.abs(afr)
                );
            }

            m.put("MATCH_1_2", match);

            return m;
        ]]>
    </java>
</stream>




public static LocalDate getLastBusinessDayOfMonth(int year, int month) {

    LocalDate date = LocalDate.of(year, month, 1)
                              .with(TemporalAdjusters.lastDayOfMonth());

    while (date.getDayOfWeek() == DayOfWeek.SATURDAY ||
           date.getDayOfWeek() == DayOfWeek.SUNDAY) {
        date = date.minusDays(1);
    }

    return date;
}
