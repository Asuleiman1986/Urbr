const COL_APPLICATION = "APPLICATION";
const COL_ASSET_TYPE = "ASSET_TYPE";

const APPLICATION_ORDER = ["xx-c", "xx-f", "xx-i"];

const APPLICATION_COLORS = {
    "xx-c": "#4FC3F7",
    "xx-f": "#81C784",
    "xx-i": "#FFB74D"
};

function generateColors(count) {
    const colors = [];

    for (let i = 0; i < count; i++) {
        const hue = Math.round((360 / Math.max(count, 1)) * i);
        colors.push(`hsl(${hue}, 70%, 55%)`);
    }

    return colors;
}

function getValue(row, columnName) {
    return row && row[columnName] != null
        ? String(row[columnName]).trim()
        : "";
}

function buildCountStats(rows, columnName, fixedOrder, colorMap) {
    const counts = new Map();

    (rows || []).forEach(row => {
        const value = getValue(row, columnName);
        if (!value) return;

        counts.set(value, (counts.get(value) || 0) + 1);
    });

    let labels;

    if (fixedOrder && fixedOrder.length) {
        labels = fixedOrder.filter(v => counts.get(v) > 0);

        Array.from(counts.keys())
            .filter(v => !fixedOrder.includes(v))
            .sort((a, b) => counts.get(b) - counts.get(a))
            .forEach(v => labels.push(v));
    } else {
        labels = Array.from(counts.keys())
            .sort((a, b) => counts.get(b) - counts.get(a));
    }

    const data = labels.map(label => counts.get(label));
    const generatedColors = generateColors(labels.length);

    const colors = labels.map((label, index) => {
        if (colorMap && colorMap[label]) return colorMap[label];
        return generatedColors[index];
    });

    return { labels, data, counts, colors };
}

function destroyChart(view, chartKey) {
    if (view[chartKey] && typeof view[chartKey].destroy === "function") {
        view[chartKey].destroy();
    }
}

function renderDonut(view, chartKey, canvasId, rows, columnName, titleId, countId, title, fixedOrder, colorMap) {
    const canvas = document.getElementById(canvasId);
    if (!canvas) return;

    const stats = buildCountStats(rows, columnName, fixedOrder, colorMap);
    const total = stats.data.reduce((a, b) => a + b, 0);

    const titleEl = document.getElementById(titleId);
    const countEl = document.getElementById(countId);

    if (titleEl) titleEl.textContent = title;
    if (countEl) countEl.textContent = total.toLocaleString();

    destroyChart(view, chartKey);

    view[chartKey] = new Chart(canvas.getContext("2d"), {
        type: "doughnut",
        data: {
            labels: stats.labels,
            datasets: [{
                data: stats.data,
                backgroundColor: stats.colors,
                borderColor: "#161616",
                borderWidth: 2
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            cutout: "55%",
            plugins: {
                legend: {
                    position: "right",
                    labels: {
                        color: "white",
                        boxWidth: 14,
                        font: { size: 12 }
                    }
                },
                tooltip: {
                    callbacks: {
                        label: function(context) {
                            const label = context.label || "";
                            const value = context.raw || 0;
                            const pct = total ? ((value / total) * 100).toFixed(1) : "0.0";
                            return `${label}: ${value.toLocaleString()} (${pct}%)`;
                        }
                    }
                },
                datalabels: {
                    display: function(context) {
                        const value = context.dataset.data[context.dataIndex];
                        const pct = total ? value / total : 0;
                        return pct >= 0.04;
                    },
                    color: "white",
                    font: {
                        size: 12,
                        weight: "bold"
                    },
                    formatter: function(value, context) {
                        const label = context.chart.data.labels[context.dataIndex];
                        const pct = total ? ((value / total) * 100).toFixed(0) : "0";
                        return `${label}\n${value.toLocaleString()}\n${pct}%`;
                    }
                }
            }
        },
        plugins: typeof ChartDataLabels !== "undefined" ? [ChartDataLabels] : []
    });
}

function getApplicationList(rows) {
    const set = new Set();

    (rows || []).forEach(row => {
        const app = getValue(row, COL_APPLICATION);
        if (app) set.add(app);
    });

    const ordered = APPLICATION_ORDER.filter(app => set.has(app));

    const others = Array.from(set)
        .filter(app => !APPLICATION_ORDER.includes(app))
        .sort((a, b) => a.localeCompare(b));

    return ordered.concat(others);
}

function renderApplicationButtons(view, tabID, table, allRows) {
    const box = document.getElementById(tabID + "-APPLICATIONButtons");
    if (!box) return;

    if (!view._selectedAPPLICATION) {
        view._selectedAPPLICATION = "ALL";
    }

    const stats = buildCountStats(allRows, COL_APPLICATION, APPLICATION_ORDER, APPLICATION_COLORS);
    const counts = stats.counts;
    const applications = getApplicationList(allRows);
    const total = (allRows || []).length;

    function makeButton(label, value, count) {
        const isActive = view._selectedAPPLICATION === value;

        const btn = document.createElement("button");
        btn.type = "button";
        btn.textContent = `${label} (${count.toLocaleString()})`;

        btn.style.cssText =
            "margin:4px; padding:7px 11px; border-radius:6px; cursor:pointer; font-weight:700;" +
            "border:1px solid #555;" +
            "background:" + (isActive ? "#2a2a2a" : "#111") + ";" +
            "color:" + (isActive ? "#fff" : "rgba(209,232,238,0.85)") + ";";

        btn.onclick = function() {
            view._selectedAPPLICATION = value;

            if (value === "ALL") {
                table.clearFilter(true);
            } else {
                table.setFilter(COL_APPLICATION, "=", value);
            }

            renderApplicationButtons(view, tabID, table, allRows);
            renderDashboardCharts(view, tabID, table.getData("active"));
        };

        return btn;
    }

    box.innerHTML = "";
    box.appendChild(makeButton("ALL", "ALL", total));

    applications.forEach(app => {
        box.appendChild(makeButton(app, app, counts.get(app) || 0));
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
