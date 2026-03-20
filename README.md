import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.TreeMap;



    private String produitWanted = "IRS";
    private String clientWanted = "CARMSECU_LCH";

    private String currentProduit = null;
    private String currentClient = null;
    private List<String> currentHeaders = new ArrayList<>();

    private boolean expectingClient = false;
    private boolean expectingHeaders = false;
    private boolean insideWantedBlock = false;

    private int resultIndex = 1;
    private TreeMap<Integer, TreeMap<String, String>> result = new TreeMap<>();

    public Cocotte() {
    }

    public Cocotte(String produitWanted, String clientWanted) {
        this.produitWanted = produitWanted;
        this.clientWanted = clientWanted;
    }

    public void setProduitWanted(String produitWanted) {
        this.produitWanted = produitWanted;
    }

    public void setClientWanted(String clientWanted) {
        this.clientWanted = clientWanted;
    }

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

        // 1) Détection produit
        if (isProduitLine(row)) {
            currentProduit = col0;
            currentClient = null;
            currentHeaders.clear();

            expectingClient = true;
            expectingHeaders = false;
            insideWantedBlock = false;
            return null;
        }

        // 2) Détection client juste après produit
        if (expectingClient) {
            currentClient = col0;
            expectingClient = false;
            expectingHeaders = true;

            insideWantedBlock = matches(currentProduit, produitWanted)
                    && matches(currentClient, clientWanted);
            return null;
        }

        // 3) Détection headers
        if (expectingHeaders) {
            currentHeaders = extractHeaders(row);
            expectingHeaders = false;
            return null;
        }

        // 4) Ignorer toutes les lignes Total
        // ex: Total: EUR, Total: HUF, Total: USD, Total: CAma, Total: IRS
        if (isTotalLine(col0)) {
            return null;
        }

        // 5) Si on n'est pas dans le bloc demandé, ignorer
        if (!insideWantedBlock) {
            return null;
        }

        // 6) Construire la ligne résultat
        TreeMap<String, String> parsedRow = new TreeMap<>();
        parsedRow.put("PRODUIT", currentProduit);
        parsedRow.put("CLIENT", currentClient);

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

    private boolean matches(String currentValue, String wantedValue) {
        if (wantedValue == null || wantedValue.trim().isEmpty()) {
            return true;
        }
        if (currentValue == null) {
            return false;
        }
        return currentValue.trim().equalsIgnoreCase(wantedValue.trim());
    }

    private boolean isProduitLine(Map row) {
        String col0 = getValue(row, "COLUMN_0");
        String col1 = getValue(row, "COLUMN_1");

        if (col0.isEmpty() || !col1.isEmpty()) {
            return false;
        }

        String v = col0.trim().toUpperCase();

        if (v.startsWith("TOTAL")) {
            return false;
        }

        return v.equals("IRS")
                || v.equals("ZCIS")
                || v.equals("CDS INDEX")
                || v.equals("CDS")
                || v.equals("TRS")
                || v.equals("FX");
    }

    private boolean isTotalLine(String value) {
        if (value == null) {
            return false;
        }
        return value.trim().toUpperCase().startsWith("TOTAL");
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
            String key = entry.getKey();
            String value = entry.getValue();

            if (!"PRODUIT".equals(key)
                    && !"CLIENT".equals(key)
                    && value != null
                    && !value.trim().isEmpty()) {
                return false;
            }
        }
        return true;
    }
}
