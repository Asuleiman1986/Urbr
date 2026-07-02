root.innerHTML = `
<div style="text-align:center;font-size:24px;font-weight:700;letter-spacing:1px;color:rgba(209,232,238,.95);margin-bottom:14px;text-transform:uppercase;">
HISTORIQUE INVENTAIRE SUMMARY - DATE|${form.DTE}
</div>

<div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:15px;margin-bottom:15px;align-items:start;">

<div style="border:1px solid #333;background:#161616;padding:12px;min-height:300px;">
<div style="color:rgba(209,232,238,.85);font-weight:700;margin-bottom:10px;">APPLICATION</div>
<div id="${tabID}-APPLICATIONButtons" style="max-height:240px;overflow:auto;"></div>
</div>

<div style="border:1px solid #333;background:#161616;padding:12px;min-height:300px;">
<div id="${tabID}-ApplicationTitle" style="color:rgba(209,232,238,.85);font-weight:700;margin-bottom:8px;">POSITIONS BY APPLICATION</div>
<div style="height:230px;"><canvas id="${tabID}-ApplicationChart"></canvas></div>
<div style="margin-top:6px;">Rows active : <b id="${tabID}-ApplicationCount"></b></div>
</div>

<div style="border:1px solid #333;background:#161616;padding:12px;min-height:300px;">
<div id="${tabID}-AssetTypeTitle" style="color:rgba(209,232,238,.85);font-weight:700;margin-bottom:8px;">POSITIONS BY ASSET TYPE</div>
<div style="height:230px;"><canvas id="${tabID}-AssetTypeChart"></canvas></div>
<div style="margin-top:6px;">Rows active : <b id="${tabID}-AssetTypeCount"></b></div>
</div>

</div>

<div id="${tabID}-Table" style="background-color:#161616"></div>
`;
    
    
    
    
    });
}

function getDashboardSuffix(view) {
    return view._selectedAPPLICATION && view._selectedAPPLICATION !== "ALL"
        ? view._selectedAPPLICATION
        : "ALL";
}

function renderDashboardCharts(view, tabID, rows) {
    const suffix = getDashboardSuffix(view);

    renderDonut(
        view,
        "_applicationChart",
        tabID + "-ApplicationChart",
        rows,
        COL_APPLICATION,
        tabID + "-ApplicationTitle",
        tabID + "-ApplicationCount",
        "POSITIONS BY APPLICATION (" + suffix + ")",
        APPLICATION_ORDER,
        APPLICATION_COLORS
    );

    renderDonut(
        view,
        "_assetTypeChart",
        tabID + "-AssetTypeChart",
        rows,
        COL_ASSET_TYPE,
        tabID + "-AssetTypeTitle",
        tabID + "-AssetTypeCount",
        "POSITIONS BY ASSET TYPE (" + suffix + ")",
        null,
        null
    );
}

******

root.innerHTML = `
<div style="text-align:center; font-size:24px; font-weight:700; letter-spacing:1px;
            color:rgba(209,232,238,0.95); margin-bottom:14px; text-transform:uppercase;">
    HISTORIQUE INVENTAIRE SUMMARY - DATE|${form.DTE}
</div>

<div style="display:flex; justify-content:center; gap:14px; margin-bottom:14px; align-items:stretch;">

    <div style="width:32%; min-height:300px; border:1px solid #333; padding:12px; background:#161616; box-sizing:border-box;">
        <div style="color:rgba(209,232,238,0.85); font-weight:700; margin-bottom:10px; text-transform:uppercase;">
            APPLICATION
        </div>
        <div id="${tabID}-APPLICATIONButtons" style="max-height:240px; overflow:auto;"></div>
    </div>

    <div style="width:32%; min-height:300px; border:1px solid #333; padding:12px; background:#161616; box-sizing:border-box;">
        <div id="${tabID}-ApplicationTitle"
             style="color:rgba(209,232,238,0.85); font-weight:700; margin-bottom:8px; text-transform:uppercase;">
            POSITIONS BY APPLICATION
        </div>
        <div style="height:230px;">
            <canvas id="${tabID}-ApplicationChart"></canvas>
        </div>
        <div style="margin-top:6px; color:rgba(209,232,238,0.75); font-size:13px;">
            Rows active: <b id="${tabID}-ApplicationCount" style="color:#fff;"></b>
        </div>
    </div>

    <div style="width:32%; min-height:300px; border:1px solid #333; padding:12px; background:#161616; box-sizing:border-box;">
        <div id="${tabID}-AssetTypeTitle"
             style="color:rgba(209,232,238,0.85); font-weight:700; margin-bottom:8px; text-transform:uppercase;">
            POSITIONS BY ASSET TYPE
        </div>
        <div style="height:230px;">
            <canvas id="${tabID}-AssetTypeChart"></canvas>
        </div>
        <div style="margin-top:6px; color:rgba(209,232,238,0.75); font-size:13px;">
            Rows active: <b id="${tabID}-AssetTypeCount" style="color:#fff;"></b>
        </div>
    </div>

</div>

<div id="${tabID}-Table" style="background-color:#161616"></div>
`;



const allRows = o.Result || [];

view._selectedAPPLICATION = "ALL";

renderApplicationButtons(view, tabID, table, allRows);
renderDashboardCharts(view, tabID, table.getData("active"));

table.on("dataFiltered", function(filters, rows) {
    const activeData = rows.map(row => row.getData());

    renderApplicationButtons(view, tabID, table, allRows);
    renderDashboardCharts(view, tabID, activeData);
});

table.on("dataChanged", function() {
    renderApplicationButtons(view, tabID, table, allRows);
    renderDashboardCharts(view, tabID, table.getData("active"));
});




*******
