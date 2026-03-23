import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.TreeMap;

public class Cocotte extends Arcturus {

    private boolean inCashSummary = false;
    private boolean expectingDescription = false;
    private boolean expectingHeaders = false;
    private boolean insideDataBlock = false;

    private String currentDescription = null;
    private List<String> currentHeaders = new ArrayList<>();

    private int resultIndex = 1;
    private TreeMap<Integer, TreeMap<String, String>> result = new TreeMap<>();

    public TreeMap<Integer, TreeMap<String, String>> getResult() {
        return result;
    }

    @Override
    public Object get(Object object) throws Exception {
        Map row = (Map) object;

        String col0 = getValue(row, "COLUMN_0");
        String col1 = getValue(row, "COLUMN_1");

        if (isEmptyRow(row)) {
            return null;
        }

        // 1. On commence seulement à partir de Cash Summary
        if (!inCashSummary) {
            if ("Cash Summary".equalsIgnoreCase(col0)) {
                inCashSummary = true;
                expectingDescription = true;
            }
            return null;
        }

        // 2. Fin totale de la section Cash Summary si on arrive sur Excess/Deficit
        if ("Excess/Deficit".equalsIgnoreCase(col0)) {
            inCashSummary = false;
            expectingDescription = false;
            expectingHeaders = false;
            insideDataBlock = false;
            currentDescription = null;
            currentHeaders.clear();
            return null;
        }

        // 3. Ligne description : LL-CARMSECU_CAS, LL-CARMSECU_CRL...
        if (expectingDescription) {
            if (!col0.isEmpty() && col1.isEmpty() && !isTotalLine(col0)) {
                currentDescription = col0;
                expectingDescription = false;
                expectingHeaders = true;
            }
            return null;
        }

        // 4. Ligne header juste après la description
        if (expectingHeaders) {
            currentHeaders = extractHeaders(row);
            expectingHeaders = false;
            insideDataBlock = true;
            return null;
        }

        // 5. Si on rencontre Total, on ferme le sous-bloc courant
        if (isTotalLine(col0)) {
            insideDataBlock = false;
            expectingDescription = true;
            expectingHeaders = false;
            currentHeaders.clear();
            currentDescription = null;
            return null;
        }

        // 6. Si on n'est pas dans un sous-bloc data, on ignore
        if (!insideDataBlock) {
            return null;
        }

        // 7. Construire la ligne résultat
        TreeMap<String, String> parsedRow = new TreeMap<>();
        parsedRow.put("DESCRIPTION", currentDescription);

        for (int i = 0; i <= 13; i++) {
            String header = getHeader(i);
            if (header == null || header.trim().isEmpty()) {
                continue;
            }

            String value = getValue(row, "COLUMN_" + i);
            parsedRow.put(header, value);
        }

        if (isOnlyMetadata(parsedRow)) {
            return null;
        }

        result.put(resultIndex++, parsedRow);
        return parsedRow;
    }

    private String getHeader(int index) {
        if (index < 0 || index >= currentHeaders.size()) {
            return "";
        }
        return currentHeaders.get(index);
    }

    private List<String> extractHeaders(Map row) {
        List<String> headers = new ArrayList<>();
        for (int i = 0; i <= 13; i++) {
            headers.add(getValue(row, "COLUMN_" + i));
        }
        return headers;
    }

    private boolean isTotalLine(String value) {
        return value != null && value.trim().toUpperCase().startsWith("TOTAL");
    }

    private String getValue(Map row, String key) {
        Object value = row.get(key);
        return value == null ? "" : value.toString().trim();
    }

    private boolean isEmptyRow(Map row) {
        for (int i = 0; i <= 13; i++) {
            Object value = row.get("COLUMN_" + i);
            if (value != null && !value.toString().trim().isEmpty()) {
                return false;
            }
        }
        return true;
    }

    private boolean isOnlyMetadata(TreeMap<String, String> row) {
        for (Map.Entry<String, String> entry : row.entrySet()) {
            if (!"DESCRIPTION".equals(entry.getKey())
                    && entry.getValue() != null
                    && !entry.getValue().trim().isEmpty()) {
                return false;
            }
        }
        return true;
    }
}
