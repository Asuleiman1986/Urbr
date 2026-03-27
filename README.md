{
  label:'Close Selected Tickets',
  onClick:function(e,tabID) {
    console.log(tabID);

    let table = Tabulator.findTable("#"+tabID+"-Result")[0];

    if (!table) {
      alert("Table not found");
      return;
    }

    let rows = table.getSelectedRows();

    if (!rows || rows.length === 0) {
      alert("No ticket selected");
      return;
    }

    let nb = rows.length;
    let msg = "Are you sure you want to close " + nb + " ticket" + (nb > 1 ? "s" : "") + " ?";

    if (!confirm(msg)) {
      return;
    }

    let promises = rows.map(row => {
      let key = row.getData()['KEY'];

      return callApis([
        {
          arcturus: 'caceis\\jirah\\closeticket',
          context: {
            KEY: key,
            TOKEN: variablesViews[tabID].param.TOKEN
          },
          parameter: {}
        }
      ])
      .then(function() {
        console.log("CLOSED TICKET: " + key);
        return { key: key, success: true };
      })
      .catch(function(err) {
        console.log("ERROR CLOSING:", key, err);
        return { key: key, success: false, error: err };
      });
    });

    Promise.all(promises).then(function(results) {
      let ok = results.filter(r => r.success).length;
      let ko = results.filter(r => !r.success).length;

      if (ko === 0) {
        alert(ok + " ticket" + (ok > 1 ? "s" : "") + " closed successfully.");
      } else {
        alert(ok + " closed / " + ko + " failed.");
      }

      // refresh si tu veux
      // loadedViews['caceis/jirah/jirah_iso_9001'].run(
      //   variablesViews[tabID].viewRef,
      //   tabID,
      //   variablesViews[tabID].param
      // );
    });
  }
}




{
  label:'Comment Tickets',
  onClick:function(e,tabID) {
    console.log(tabID);

    let table = Tabulator.findTable("#"+tabID+"-Result")[0];

    if (!table) {
      alert("Table not found");
      return;
    }

    let rows = table.getSelectedRows();

    if (!rows || rows.length === 0) {
      alert("No ticket selected");
      return;
    }

    let comment = prompt("Write the comment to add in JIRAH:");

    if (comment === null) {
      return;
    }

    comment = comment.trim();

    if (comment.length === 0) {
      alert("Comment is empty");
      return;
    }

    let nb = rows.length;
    let msg = "Are you sure you want to add this comment to " + nb + " ticket" + (nb > 1 ? "s" : "") + " ?";

    if (!confirm(msg)) {
      return;
    }

    let promises = rows.map(row => {
      let key = row.getData()['KEY'];

      return callApis([
        {
          arcturus: 'caceis\\jirah\\commentTicket',
          context: {
            KEY: key,
            TOKEN: variablesViews[tabID].param.TOKEN,
            COMMENT: comment
          },
          parameter: {}
        }
      ])
      .then(function() {
        console.log("COMMENT ADDED:", key);
        return { key: key, success: true };
      })
      .catch(function(err) {
        console.log("ERROR COMMENTING:", key, err);
        return { key: key, success: false, error: err };
      });
    });

    Promise.all(promises).then(function(results) {
      let ok = results.filter(r => r.success).length;
      let ko = results.filter(r => !r.success).length;

      if (ko === 0) {
        alert("Comment added in JIRAH ticket" + (ok > 1 ? "s" : "") + ".");
      } else {
        alert("Comment added on " + ok + " ticket" + (ok > 1 ? "s" : "") + " / " + ko + " failed.");
      }

      // refresh si besoin
      // loadedViews['caceis/jirah/jirah_iso_9001'].run(
      //   variablesViews[tabID].viewRef,
      //   tabID,
      //   variablesViews[tabID].param
      // );
    });
  }
}
