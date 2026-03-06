import java.nio.file.Files;
import java.nio.file.Path;
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.util.HashSet;
import java.util.Set;



private static final String ICS_FILE_PATH = "C:/file.ics";

public static Set<LocalDate> loadHolidaysFromIcs() {
    Set<LocalDate> holidays = new HashSet<>();

    try {
        for (String line : Files.readAllLines(Path.of(ICS_FILE_PATH))) {
            if (line.startsWith("DTSTART")) {
                String date = line.substring(line.indexOf(":") + 1).trim();

                if (date.length() >= 8) {
                    date = date.substring(0, 8);
                    holidays.add(LocalDate.parse(date, DateTimeFormatter.BASIC_ISO_DATE));
                }
            }
        }
    } catch (Exception e) {
        // en cas d'erreur, on retourne juste un set vide
    }

    return holidays;
}

Set<LocalDate> holidays = loadHolidaysFromIcs();
