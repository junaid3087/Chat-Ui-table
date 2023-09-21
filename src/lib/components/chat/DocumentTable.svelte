<!-- DocumentTable.svelte -->
<script>
	import ChatInput from "./ChatInput.svelte";
	import UploadButton from "./UploadButton.svelte";

	/**
	 * @type {any[]}
	 */
	export let documentData = []; // Pass your document data array as a prop
	// @ts-ignore
	let buttonRef: HTMLDivElement;
	let isModalOpen = false;
	/**
	 * @param {number} index
	 */
	function deleteDocument(index) {
		// Implement document deletion logic here
		documentData.splice(index, 1); // Remove the document at the specified index
	}
	function handleButtonClick(event: MouseEvent) {
		// Prevent the click event from propagating to the document-level click handler
		event.stopPropagation();

		isModalOpen = true; // Open the modal dialog
	}
</script>

<div>
	<ChatInput />
	<table>
		<thead>
			<tr>
				<th>Docr</th>
				<th>Date</th>
				<th>Delete</th>
			</tr>
		</thead>
		<tbody>
			{#each documentData as document, index (index)}
				<tr>
					<td>{document.name}</td>
					<td>{document.date}</td>
					<td><button on:click={() => deleteDocument(index)}>Delete</button></td>
				</tr>
			{/each}
		</tbody>
	</table>
</div>

<style>
	/* Add custom styles for the table cells */
	table {
		width: 100%;
		border-collapse: collapse;
	}

	th,
	td {
		padding: 8px;
		text-align: left;
		border-bottom: 1px solid #ddd;
	}

	th {
		background-color: #f2f2f2;
	}

	/* Darken the font color */
	td {
		color: #333; /* Darker font color */
	}

	button {
		background-color: transparent;
		border: none;
		cursor: pointer;
		color: #3490dc; /* Button color (you can adjust this as needed) */
	}
</style>
