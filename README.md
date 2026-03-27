          
      let root = document.getElementById(tabID + "-Result");

root.innerHTML = `
  <div style="
    text-align:center;
    font-size:26px;
    font-weight:700;
    color:white;
    margin-bottom:10px;
  ">
    RECONCILIATION DASHBOARD
  </div>

  <div id="${tabID}-Table"></div>
`;



root.innerHTML = `
  <div style="
    text-align:center;
    padding:12px;
    background:#1e1e1e;
    border-bottom:1px solid #333;
    margin-bottom:10px;
  ">
    <div style="font-size:24px; font-weight:700; color:#fff;">
      RECONCILIATION DASHBOARD
    </div>
    <div style="font-size:14px; color:#aaa;">
      ${form?.PORTEFEUILLE || ""}
    </div>
  </div>

  <div id="${tabID}-Table"></div>
`;

new Tabulator("#"+tabID+"-Table", {
       
       
       
       
       
       
       
       
       
       
       root.innerHTML = `
  <div style="
    text-align:center;
    padding:15px;
    background:#1e1e1e;
    border-bottom:2px solid #2c2c2c;
    margin-bottom:15px;
  ">
    <div style="font-size:26px; font-weight:700; color:#fff;">
      RECONCILIATION DASHBOARD
    </div>

    <div style="font-size:16px; color:#aaa;">
      ${form.PORTEFEUILLE} - Cash Positions
    </div>
  </div>
`;                 
