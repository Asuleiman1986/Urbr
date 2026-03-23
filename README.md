import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.TreeMap;


    private boolean cashSummarySeen = false;
    private boolean inTargetCashSummary = false;
    private boolean stopParsing = false;

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
        if (stopParsing) {
            return null;
        }

        Map row = (Map) object;
        String col0 = getValue(row, "COLUMN_0");
        String col1 = getValue(row, "COLUMN_1");

        if (isEmptyRow(row)) {
            return null;
        }

        // Fin de la section cible
        if ("Excess/Deficit".equalsIgnoreCase(col0)) {
            if (inTargetCashSummary) {
                stopParsing = true;
            }
            resetCurrentSubBlock();
            return null;
        }

        // Détection d'un Cash Summary
        if ("Cash Summary".equalsIgnoreCase(col0)) {
            cashSummarySeen = true;
            inTargetCashSummary = false;
            resetCurrentSubBlock();
            expectingDescription = true;
            return null;
        }

        // Tant qu'on n'a pas vu de Cash Summary, on ignore
        if (!cashSummarySeen) {
            return null;
        }

        // Ligne description
        if (expectingDescription) {
            if (!col0.isEmpty() && col1.isEmpty() && !isTotalLine(col0)) {
                currentDescription = col0;
                expectingDescription = false;
                expectingHeaders = true;
            }
            return null;
        }

        // Ligne header
        if (expectingHeaders) {
            currentHeaders = extractHeaders(row);

            // On démarre seulement si ce header contient "Opening"
            if (containsHeader(currentHeaders, "T Opening Balance")) {
                inTargetCashSummary = true;
                insideDataBlock = true;
            } else {
                // mauvais Cash Summary ou mauvais sous-bloc
                insideDataBlock = false;
            }

            expectingHeaders = false;
            return null;
        }

        // Ligne Total => fin du sous-bloc courant
        if (isTotalLine(col0)) {
            insideDataBlock = false;
            currentDescription = null;
            currentHeaders.clear();

            // Tant qu'on est dans la bonne section Cash Summary,
            // on peut attendre un autre sous-bloc
            if (!stopParsing) {
                expectingDescription = true;
            }
            return null;
        }

        // Si on n'est pas dans le bon Cash Summary, on ignore
        if (!inTargetCashSummary) {
            return null;
        }

        // Si on n'est pas dans un bloc de données, on ignore
        if (!insideDataBlock) {
            return null;
        }

        // Construire la ligne résultat
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

        if (hasNoData(parsedRow)) {
            return null;
        }

        result.put(resultIndex++, parsedRow);
        return parsedRow;
    }

    private void resetCurrentSubBlock() {
        expectingDescription = false;
        expectingHeaders = false;
        insideDataBlock = false;
        currentDescription = null;
        currentHeaders.clear();
    }

    private boolean containsHeader(List<String> headers, String searchedText) {
        if (headers == null || searchedText == null) {
            return false;
        }

        for (String header : headers) {
            if (header != null && header.toUpperCase().contains(searchedText.toUpperCase())) {
                return true;
            }
        }
        return false;
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

    private boolean hasNoData(TreeMap<String, String> row) {
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
