public static LocalDate getLastBusinessDayOfMonth(int year, int month) {

    LocalDate date = LocalDate.of(year, month, 1)
                              .with(TemporalAdjusters.lastDayOfMonth());

    while (date.getDayOfWeek() == DayOfWeek.SATURDAY ||
           date.getDayOfWeek() == DayOfWeek.SUNDAY) {
        date = date.minusDays(1);
    }

    return date;
}
