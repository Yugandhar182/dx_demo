<script>
	import { onMount } from "svelte";
	import DevExpress from "devextreme";
  
	let jsonData = [];
	let data = [];
  
	onMount(async () => {
	  const response = await fetch(
		"https://api.recruitly.io/api/candidate?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E"
	  );
	  const responseData = await response.json();
	  jsonData = responseData.data;
	  console.log(jsonData, "json");
  
	  const gridData = jsonData
		.slice(0, jsonData.length - 5)
		.map((item) => ({
		
		  firstname: item.firstname,
		  surname: item.surname,
		  email: item.email,
		  mobile: item.mobile,
		}));
  
	  console.log(gridData, "griddata");
  
	  const dataGrid = new DevExpress.ui.dxDataGrid(
		document.getElementById("dataGrid"),
		{
		  dataSource: gridData,
		  columns: [
			
			{ dataField: "firstname", caption: "First Name" },
			{ dataField: "surname", caption: "Surname" },
			{ dataField: "email", caption: "Email" },
			{ dataField: "mobile", caption: "Mobile" },
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
			onRowInserted: async (e) => {
			  const newRowData = e.data;
			  try {
				const response = await fetch(
				  "https://api.recruitly.io/api/candidate?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E",
				  {
					method: "POST",
					headers: {
					  "Content-Type": "application/json",
					},
					body: JSON.stringify(newRowData),
				  }
				);
  
				if (response.ok) {
				  // Handle success
				  console.log("New row added successfully");
				} else {
				  // Handle error
				  console.error("Failed to add new row");
				}
			  } catch (error) {
				// Handle error
				console.error("Failed to add new row", error);
			  }
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
		}
	  );
	});
  </script>
  
  <div id="dataGrid"></div>
  