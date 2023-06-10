<script>
	// Sample data
	let jsonData = [];
	let data = [];
  
	onMount(async () => {
	  const response = await fetch(
		"https://api.recruitly.io/api/candidate?apiKey=TEST9349C0221517DA4942E39B5DF18C68CDA154"
	  );
	  const responseData = await response.json();
	  jsonData = responseData.data;
	  console.log(jsonData, "json");
  
	  const gridData = jsonData.map((item) => ({
		reference: item.reference,
		name: item.fullName,
		email: item.email,
		phone: item.mobile,
	  }));
  
	  const dataGrid = new DevExpress.ui.dxDataGrid("#dataGrid", {
		dataSource: gridData,
		columns: [
		  { dataField: "reference", caption: "ID" },
		  { dataField: "name", caption: "Name", dataType: "url" },
		  { dataField: "email", caption: "Email" },
		  { dataField: "phone", caption: "Mobile" },
		],
		showBorders: true,
		filterRow: {
		  visible: true,
		},
		editing: {
		  allowDeleting: true,
		  allowAdding: true,
		  allowUpdating: true,
		  mode: "popup",
		  form: {
			labelLocation: "top",
		  },
		  popup: {
			showTitle: true,
			title: "Row in the editing state",
		  },
		  onRowInserted: function (e) {
			// Call a function to save the added row to the API
			saveRowToAPI(e.data);
		  },
		},
		paging: {
		  pageSize: 10,
		},
		pager: {
		  showPageSizeSelector: true,
		  allowedPageSizes: [5, 10, 20],
		  showInfo: true,
		},
	  });
  
	  function saveRowToAPI(rowData) {
		fetch("https://api.recruitly.io/api/candidate?apiKey=TEST9349C0221517DA4942E39B5DF18C68CDA154", {
		  method: "POST",
		  headers: {
			"Content-Type": "application/json",
		  },
		  body: JSON.stringify(rowData),
		})
		  .then((response) => {
			if (response.ok) {
			  console.log("Row data saved successfully!");
			  // Refresh the data grid after successful save
			  dataGrid.refresh();
			} else {
			  console.error("Failed to save row data:", response.status);
			}
		  })
		  .catch((error) => {
			console.error("An error occurred while saving row data:", error);
		  });
	  }
	});
  </script>
  