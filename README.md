root.innerHTML = `
  <div style="text-align:center; font-size:24px; font-weight:700; letter-spacing:1px; color:rgba(209,232,238,0.95); margin-bottom:10px; text-transform:uppercase;">
    CASH POSITIONS CARMIGNAC - ${form.PORTEFEUILLE} RECONCILIATION
  </div>

  <div style="display:flex; justify-content:center; gap:14px; margin-bottom:14px; align-items:stretch;">

    <!-- COLONNE GAUCHE -->
    <div style="width:520px; height:320px; display:flex; flex-direction:column; gap:10px;">

      <!-- BROKERS -->
      <div style="flex:1; border:1px solid #333; padding:10px; background:#161616; box-sizing:border-box;">
        <div style="color:rgba(209,232,238,0.85); font-weight:600; margin-bottom:8px; text-transform:uppercase;">
          Brokers
        </div>

        <div id="${tabID}-BrokerButtons" style="max-height:120px; overflow:auto;"></div>

        <div style="margin-top:8px; color:#ff4d4f; font-size:12px;">
          Clé de matching : ACCOUNT - BROKER - ISIN - FUND <br>
          Exception SG et ML ou la clé de matching est ACCOUNT - BROKER - FUND
        </div>
      </div>

      <!-- AMOUNTS SUMMARY -->
      <div id="${tabID}-AmountsSummary"
           style="height:78px; border:1px solid #333; padding:10px 14px; background:#161616; box-sizing:border-box; color:white; display:flex; align-items:center;">
      </div>

    </div>

    <!-- COLONNE DROITE -->
    <div style="width:520px; height:320px; border:1px solid #333; padding:10px; background:#161616; box-sizing:border-box;">
      <div id="${tabID}-StatusTitle"
           style="color:rgba(209,232,238,0.85); font-weight:600; margin-bottom:8px; text-transform:uppercase;">
        COUNT BY STATUS (ALL)
      </div>

      <div style="height:240px;">
        <canvas id="${tabID}-StatusChart"></canvas>
      </div>

      <div style="margin-top:6px; color:rgba(209,232,238,0.75); font-size:13px;">
        Rows (active): <b id="${tabID}-StatusCount" style="color:#fff;"></b>
      </div>
    </div>

  </div>

  <div id="${tabID}-Table" style="background-color:#161616"></div>
`;





function renderAmountsSummary(view, tabID, rows){
    if (!Array.isArray(rows)) return;

    let sumGP = 0;
    let sumBroker = 0;

    rows.forEach(r => {
        sumGP += Number(r.AMOUNT_GP) || 0;
        sumBroker += Number(r.AMOUNT_BROKER) || 0;
    });

    const diff = sumGP - sumBroker;

    const el = document.getElementById(tabID + "-AmountsSummary");
    if (!el) return;

    el.innerHTML = `
        <div style="width:100%; display:flex; align-items:center; justify-content:space-between; gap:14px; font-size:14px;">
            
            <div style="display:flex; flex-direction:column; min-width:0;">
                <div style="font-size:13px; color:rgba(209,232,238,0.75); text-transform:uppercase;">GP</div>
                <div style="font-size:20px; font-weight:700; color:#ffffff;">${sumGP.toLocaleString()}</div>
            </div>

            <div style="width:1px; align-self:stretch; background:#3a3a3a;"></div>

            <div style="display:flex; flex-direction:column; min-width:0;">
                <div style="font-size:13px; color:rgba(209,232,238,0.75); text-transform:uppercase;">Broker</div>
                <div style="font-size:20px; font-weight:700; color:#ffffff;">${sumBroker.toLocaleString()}</div>
            </div>

            <div style="width:1px; align-self:stretch; background:#3a3a3a;"></div>

            <div style="display:flex; flex-direction:column; min-width:0;">
                <div style="font-size:13px; color:rgba(209,232,238,0.75); text-transform:uppercase;">Diff</div>
                <div style="font-size:20px; font-weight:700; color:${diff === 0 ? '#71B86F' : '#8fd19e'};">
                    ${diff > 0 ? '+' : ''}${diff.toLocaleString()}
                </div>
            </div>

        </div>
    `;
}
