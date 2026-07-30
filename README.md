 import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class IcsReader {

    public static void lireFichierICS(String cheminFichier) {

        try (BufferedReader reader = new BufferedReader(new FileReader(cheminFichier))) {

            String ligne;
            String summary = "";
            String date = "";

            while ((ligne = reader.readLine()) != null) {

                // Nom de l'événement
                if (ligne.startsWith("SUMMARY:")) {
                    summary = ligne.substring("SUMMARY:".length());
                }

                // Date de début
                if (ligne.startsWith("DTSTART")) {
                    date = ligne.substring(ligne.indexOf(":") + 1);
                }

                // Fin de l'événement : on affiche les informations
                if (ligne.equals("END:VEVENT")) {
                    System.out.println("--------------------------------");
                    System.out.println("Nom  : " + summary);
                    System.out.println("Date : " + date);
                }
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {

        String chemin = "C:\\Users\\ASULEIMAN\\Downloads\\Monaco_2027.ics";

        lireFichierICS(chemin);
    }
}
