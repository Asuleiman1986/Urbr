# Urbrimport java.time.*;
import java.time.format.DateTimeFormatter;
import java.util.HashSet;
import java.util.Set;

public class BusinessTimeCalculator {

    private static final DateTimeFormatter JIRA_FORMAT =
            DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm:ss.SSSZ");

    private static final LocalTime WORK_START = LocalTime.of(8, 0);
    private static final LocalTime WORK_END   = LocalTime.of(19, 0);

    public static LocalDateTime parseJiraDate(String s) {
        if (s == null || s.trim().isEmpty()) {
            return null;
        }
        return OffsetDateTime.parse(s.trim(), JIRA_FORMAT).toLocalDateTime();
    }

    public static boolean isHoliday(LocalDate date, Set<LocalDate> holidays) {
        return holidays != null && holidays.contains(date);
    }

    public static boolean isBusinessDay(LocalDate date, Set<LocalDate> holidays) {
        DayOfWeek dow = date.getDayOfWeek();
        if (dow == DayOfWeek.SATURDAY || dow == DayOfWeek.SUNDAY) {
            return false;
        }
        return !isHoliday(date, holidays);
    }

    /**
     * Si la date de début est hors heures ouvrées, week-end ou jour férié,
     * on la décale au prochain jour ouvré à 08:00.
     */
    public static LocalDateTime normalizeStart(LocalDateTime dt, Set<LocalDate> holidays) {
        if (dt == null) return null;

        LocalDate date = dt.toLocalDate();
        LocalTime time = dt.toLocalTime();

        while (!isBusinessDay(date, holidays)) {
            date = date.plusDays(1);
        }

        if (time.isBefore(WORK_START)) {
            return date.atTime(WORK_START);
        }

        if (!time.isBefore(WORK_END)) {
            date = date.plusDays(1);
            while (!isBusinessDay(date, holidays)) {
                date = date.plusDays(1);
            }
            return date.atTime(WORK_START);
        }

        return dt;
    }

    /**
     * Si la date de fin est hors heures ouvrées, week-end ou jour férié,
     * on la ramène à la dernière borne ouvrée valable.
     */
    public static LocalDateTime normalizeEnd(LocalDateTime dt, Set<LocalDate> holidays) {
        if (dt == null) return null;

        LocalDate date = dt.toLocalDate();
        LocalTime time = dt.toLocalTime();

        while (!isBusinessDay(date, holidays)) {
            date = date.minusDays(1);
        }

        if (time.isBefore(WORK_START)) {
            date = date.minusDays(1);
            while (!isBusinessDay(date, holidays)) {
                date = date.minusDays(1);
            }
            return date.atTime(WORK_END);
        }

        if (time.isAfter(WORK_END)) {
            return date.atTime(WORK_END);
        }

        return dt;
    }

    public static double businessHoursBetween(String startStr, String endStr, Set<LocalDate> holidays) {
        try {
            LocalDateTime start = parseJiraDate(startStr);
            LocalDateTime end   = parseJiraDate(endStr);

            return businessHoursBetween(start, end, holidays);

        } catch (Exception e) {
            return 0.0;
        }
    }

    public static double businessHoursBetween(LocalDateTime start, LocalDateTime end, Set<LocalDate> holidays) {
        try {
            if (start == null || end == null) {
                return 0.0;
            }

            start = normalizeStart(start, holidays);
            end   = normalizeEnd(end, holidays);

            if (start == null || end == null || !end.isAfter(start)) {
                return 0.0;
            }

            long totalMinutes = 0;

            LocalDate currentDate = start.toLocalDate();
            LocalDate endDate = end.toLocalDate();

            while (!currentDate.isAfter(endDate)) {

                if (isBusinessDay(currentDate, holidays)) {
                    LocalDateTime dayWorkStart = currentDate.atTime(WORK_START);
                    LocalDateTime dayWorkEnd   = currentDate.atTime(WORK_END);

                    LocalDateTime effectiveStart = max(start, dayWorkStart);
                    LocalDateTime effectiveEnd   = min(end, dayWorkEnd);

                    if (effectiveEnd.isAfter(effectiveStart)) {
                        totalMinutes += Duration.between(effectiveStart, effectiveEnd).toMinutes();
                    }
                }

                currentDate = currentDate.plusDays(1);
            }

            return totalMinutes / 60.0;

        } catch (Exception e) {
            return 0.0;
        }
    }

    private static LocalDateTime max(LocalDateTime a, LocalDateTime b) {
        return a.isAfter(b) ? a : b;
    }

    private static LocalDateTime min(LocalDateTime a, LocalDateTime b) {
        return a.isBefore(b) ? a : b;
    }

    public static void main(String[] args) {
        Set<LocalDate> holidays = new HashSet<>();

        holidays.add(LocalDate.of(2024, 1, 1));
        holidays.add(LocalDate.of(2024, 4, 1));
        holidays.add(LocalDate.of(2024, 5, 1));
        holidays.add(LocalDate.of(2024, 5, 8));
        holidays.add(LocalDate.of(2024, 12, 25));

        String created = "2024-05-01T10:00:00.000+0100";
        String closed  = "2024-05-02T10:30:00.000+0100";

        double hours = businessHoursBetween(created, closed, holidays);

        System.out.println("Business hours = " + hours);
    }
}
