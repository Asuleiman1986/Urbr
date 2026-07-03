function buildTicketAction(row) {

    let key = row.KEY;

    let appli = (row.APPLICATION || "").toLowerCase().trim();
    let summary = (row.SUMMARY || "").toLowerCase().trim();
    let description = (row.DESCRIPTION || "").toLowerCase().trim();

    let comment = null;
    let status = null;

    // =====================================================
    // APPLICATION
    // =====================================================

    if (appli === "gp3-rappro" || appli === "gpc-rappro") {

        comment = "Veuillez joindre le document de Rappro en PJ";
        status = "En attente de complément d'information";

    } else if (appli === "gp3-crater" || appli === "gpc-crater") {

        comment = "Veuillez joindre la capture d'écran CRATER en PJ";
        status = "En attente de complément d'information";

    } else if (appli === "gp3-rapide" || appli === "gpc-rapide") {

        comment =
            "Veuillez préciser le numéro de transaction et de portefeuille. " +
            "Interdiction d'ajouter des pièces jointes, sauf en cas confirmé de demande en masse.";

        status = "En attente de complément d'information";

    } else if (appli === "olis fa-rapide") {

        comment = "Veuillez joindre le document SUIVL en PJ";
        status = "En attente de complément d'information";

    } else if (appli === "frais de gestion variables") {

        comment =
            "Veuillez fournir les éléments suivants :\n\n" +
            "- Capture d'écran de la macro de contrôle\n" +
            "- Lien vers le fichier répertorié sur le réseau\n" +
            "- Indication de la part concernée\n" +
            "- Date de VL depuis laquelle l'écart est constaté.";

        status = "En attente de complément d'information";

    } else if (appli === "fastcheck") {

        comment =
            "Veuillez joindre une capture d'écran de l'anomalie (Croix rouges / grises / sablier).";

        status = "En attente de complément d'information";
    }

    // =====================================================
    // SUMMARY
    // =====================================================

    if (summary.includes("fastmatch")) {

        status = "Fermée";
    }

    if (summary.includes("tribank")) {

        comment = "Veuillez diriger votre demande vers le Jira Tribank";
        status = "Fermée";
    }

    if (summary.includes("swift") && summary.includes("flux")) {

        comment = "Veuillez diriger votre demande vers le Jira Flux";
        status = "Fermée";
    }

    if (summary.includes("gp lux")) {

        comment = "Veuillez diriger votre demande vers OPSU";
        status = "Fermée";
    }

    if (
        summary.includes("custodian") &&
        (
            summary.includes("flux") ||
            summary.includes("stuck") ||
            summary.includes("bloqué") ||
            summary.includes("olis")
        )
    ) {

        comment = "Veuillez-vous adresser à votre Relationship Manager";
        status = "Fermée";
    }

    if (
        summary.includes("détruire") &&
        (
            summary.includes("parhdg") ||
            summary.includes("acthdg")
        )
    ) {

        comment = "Vous disposez de la main pour effectuer l'action demandée";
        status = "Fermée";
    }

    if (
        summary.includes("rattach") &&
        summary.includes("hedge")
    ) {

        status = "Fermée";
    }

    if (
        summary.includes("déséquilibre") &&
        summary.includes("parts hedgées")
    ) {

        comment = "Veuillez joindre le RAPPROGC et le LACTHDG";
        status = "Informations";
    }

    // =====================================================
    // DESCRIPTION
    // =====================================================

    if (
        description.includes("swift") &&
        description.includes("fastmatch")
    ) {

        comment = "Veuillez diriger votre demande vers le Jira MIDL";
        status = "Fermée";
    }

    if (
        description.includes("custodian") &&
        (
            description.includes("flux") ||
            description.includes("stuck") ||
            description.includes("bloqué") ||
            description.includes("olis")
        )
    ) {

        comment = "Veuillez-vous adresser à votre Relationship Manager";
        status = "Fermée";
    }

    if (
        description.includes("détruire") &&
        (
            description.includes("parhdg") ||
            description.includes("acthdg")
        )
    ) {

        comment = "Vous disposez de la main pour effectuer l'action demandée";
        status = "Fermée";
    }

    if (
        description.includes("rattach") &&
        description.includes("hedge")
    ) {

        status = "Fermée";
    }

    if (
        description.includes("déséquilibre") &&
        description.includes("parts hedgées")
    ) {

        comment = "Veuillez joindre le RAPPROGC et le LACTHDG";
        status = "Informations";
    }

    // =====================================================
    // Aucune action
    // =====================================================

    if (comment === null && status === null) {
        return null;
    }

    return {
        key: key,
        comment: comment,
        status: status
    };
}
