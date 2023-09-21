<!-- ChatInput.svelte -->
<script lang="ts">
	import { onMount, onDestroy } from "svelte";

	let isModalOpen = false;
	let uploadInputRef: HTMLInputElement;
	let buttonRef: HTMLDivElement; // Reference to the "+" button element

	// Function to trigger the opening of the dialog box
	function handleButtonClick(event: MouseEvent) {
		// Prevent the click event from propagating to the document-level click handler
		event.stopPropagation();

		isModalOpen = true; // Open the modal dialog
	}

	// Function to close the modal dialog
	function closeModal() {
		isModalOpen = false;
	}

	// Function to handle file selection and upload
	function handleFileUpload() {
		const input = uploadInputRef;
		if (input) input.click(); // Open the file picker
	}

	function handleFileInputChange(event: Event) {
		const inputElement = event.target as HTMLInputElement;
		const files = inputElement.files;

		if (files && files.length > 0) {
			const fileList = Array.from(files); // Corrected syntax here
			dispatch("fileUpload", fileList);
		}

		// Clear the file input value to allow selecting the same file again
		inputElement.value = "";
	}

	// Function to handle clicks outside the modal (only in the browser environment)
	function clickOutsideHandler(event: MouseEvent) {
		const modal = document.querySelector(".modal");
		if (modal && !modal.contains(event.target as Node)) {
			closeModal();
		}
	}

	onMount(() => {
		// Add the click outside handler when the component is mounted (only in the browser environment)
		if (typeof window !== "undefined") {
			window.addEventListener("click", clickOutsideHandler);
		}
	});

	onDestroy(() => {
		// Cleanup the click outside handler when the component is destroyed (only in the browser environment)
		if (typeof window !== "undefined") {
			window.removeEventListener("click", clickOutsideHandler);
		}
	});
</script>

<div class="relative">
	<!-- "+ Upload" button -->
	<button class="btn-upload-document" on:click={handleButtonClick} bind:this={buttonRef}>
		+
	</button>
	<!-- Modal for file upload -->
	<div class="modal" style="display: {isModalOpen ? 'block' : 'none'}; top: -80px; left: 0;">
		<div class="modal-content">
			<!-- Make "Upload File" text clickable -->
			<h2 style="cursor: pointer; margin-bottom: 10px;" on:click={handleFileUpload}>Upload File</h2>
			<p>PDF, DOC, DOCX</p>
		</div>
	</div>
</div>

<input
	type="file"
	id="file-upload-input"
	accept=".pdf,.doc,.docx,.txt"
	style="display: none;"
	bind:this={uploadInputRef}
	on:change={handleFileInputChange}
/>

<style>
	/* Styles for your button */
	.btn-upload-document {
		cursor: pointer;
		background-color: #3490dc;
		color: #ffffff;
		padding: 6px 10px;
		border: none;
		border-radius: 4px;
		font-size: 18px;
	}

	.btn-upload-document:hover {
		background-color: #2779bd;
	}

	/* Smaller Modal Styles */
	.modal {
		display: none;
		position: absolute;
		background-color: #fff;
		border: 1px solid #ccc;
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
		border-radius: 10px; /* Make it rounder */
		z-index: 1;
	}

	.modal-content {
		padding: 10px;
		text-align: center;
		color: #999; /* Lighter font color */
		font-weight: lighter; /* Lighter font weight */
	}
</style>
