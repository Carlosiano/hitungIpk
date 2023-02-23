<script lang="ts">
	function addRow() {
		data = [...data, [...newRow]];
		newRow = ['', '', ''];
	}
	function deleteRow(rowToBeDeleted: any, index: any) {
		data = data.filter((row) => row != rowToBeDeleted);
		totalSks.splice(index, 1);
		totalNilai.splice(index, 1);
		ipk.splice(index, 1);
	}
	let columns = ['Mata Kuliah', 'SKS', 'Nilai Huruf', 'Aksi'];
	let data: any[] = [];
	let newRow = ['', '', ''];

	let totalMatkul: number;
	let totalSks: number[] = [];
	let totalNilai: number[] = [];
	let ipk: number[] = [];

	$: data.forEach((item, index) => {
		totalSks[index] = Number(item[1]);
		totalNilai[index] = Number(item[2]);
		ipk[index] = totalSks[index] * totalNilai[index];
		totalMatkul = index + 1;
	});

	addRow();
</script>

<div class="container">
	<h2>Kalkulator IPK(by Carlosiano)</h2>

	<table>
		<tr>
			{#each columns as column}
				<th>{column}</th>
			{/each}
		</tr>

		{#each data as row, index}
			<tr>
				{#each row as cell, i}
					<td>
						{#if i == 1}
							<select name="sks" id="sks" bind:value={cell}>
								<option value="2">2</option>
								<option value="4">4</option>
							</select>
						{:else if i == 2}
							<select name="nilai" id="nilai" bind:value={cell}>
								<option value="4">A</option>
								<option value="3">B</option>
								<option value="2">C</option>
								<option value="1">D</option>
								<option value="0">E</option>
							</select>
						{:else}
							<input type="text" bind:value={cell} />
						{/if}
					</td>
				{/each}
				<button on:click={() => deleteRow(row, index)}>X</button>
			</tr>
		{/each}
		<tr style="color: grey">
			{#each newRow as column}
				<td contenteditable="false" bind:innerHTML={column} />
			{/each}
			<button on:click={addRow}>add</button>
		</tr>
	</table>

	<div class="ipk">
		<div class="total-sks">Total SKS: {totalSks.reduce((partialSum, a) => partialSum + a, 0)}</div>
		<div class="total-matkul">Total Mata Kuliah: {totalMatkul}</div>
		<div class="nilai-ipk">
			IPK: {data.length == 0
				? '0'
				: ipk.reduce((partialSum, a) => partialSum + a, 0) /
				  totalSks.reduce((partialSum, a) => partialSum + a, 0)}
		</div>
	</div>
</div>

<style>
	table,
	th,
	td {
		border: 1px solid black;
	}
  .container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
</style>
