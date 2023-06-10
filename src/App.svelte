<script>
	import { onMount } from "svelte";
	import DevExpress from "devextreme";
	
	let jsonData = [];
	
	onMount(async () => {
	  const response = await fetch(
		"https://api.recruitly.io/api/candidate?apiKey=TEST9349C0221517DA4942E39B5DF18C68CDA154"
	  );
	  const responseData = await response.json();
	  jsonData = responseData.data;
	
	  const gridData = jsonData.map((item) => ({
		id: item.reference,
		firstName: item.fullName,
		surname: item.fullName,
		email: item.email,
		mobile: item.mobile,
	  }));
	
	  createDataGrid(gridData);
	});
	
	function createDataGrid(gridData) {
	  const dataGrid = new DevExpress.ui.dxDataGrid(
		document.getElementById("dataGrid"),
		{
		  dataSource: gridData,
		  columns: [
			{ dataField: "id", caption: "ID" },
			{ dataField: "firstName", caption: "First Name" },
			{ dataField: "surname", caption: "Surname" },
			{ dataField: "email", caption: "Email" },
			{ dataField: "mobile", caption: "Mobile" },
		  ],
		  showBorders: true,
		  filterRow: {
			visible: true,
		  },
		  editing: {
			allowAdding: false, // Disable adding new rows
			allowUpdating: false, // Disable updating existing rows
			allowDeleting: false, // Disable deleting rows
		  },
		  paging: {
			pageSize: 10,
		  },
		  pager: {
			showPageSizeSelector: true,
			allowedPageSizes: [5, 10, 20],
			showInfo: true,
		  },
		}
	  );
	}
  </script>
  
  <div id="dataGrid"></div>
  