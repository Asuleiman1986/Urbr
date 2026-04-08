          
<div id="${tabID}-AmountsSummary"
     style="margin-top:10px; padding:10px; background:#161616; border:1px solid #333; color:white;">
</div>


function renderAmountsSummary(tabID, rows){
    let sumGP = 0;
    let sumBroker = 0;

    (rows || []).forEach(r => {
        sumGP += Number(r.AMOUNT_GP) || 0;
        sumBroker += Number(r.AMOUNT_BROKER) || 0;
    });

    const diff = sumGP - sumBroker;

    const el = document.getElementById(tabID + "-AmountsSummary");
    if(!el) return;

    el.innerHTML = `
        <div style="display:flex; justify-content:space-around; font-size:14px;">
            <div>💰 GP: <b>${sumGP.toLocaleString()}</b></div>
            <div>🏦 Broker: <b>${sumBroker.toLocaleString()}</b></div>
            <div>⚖️ Diff: <b style="color:${diff===0?'#71B86F':'#ff4d4f'}">
                ${diff.toLocaleString()}
            </b></div>
        </div>
    `;
}






renderAmountsSummary(tabID, table.getData("active"));



table.on("dataFiltered", function(filters, rows){
    const activeData = rows.map(r => r.getData());

    renderStatusDonut(view, tabID, activeData, suffix);
    renderAmountsSummary(tabID, activeData);   // 👈 AJOUT
});




renderAmountsSummary(tabID, table.getData("active"));


renderAmountsSummary(tabID, table.getData("active"));
