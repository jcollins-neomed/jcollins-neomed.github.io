<select id="categorySelect">
  <option value="">--Choose a dominant city--</option>
  <option value="Cleveland">Cleveland</option>
  <option value="Akron">Akron</option>
  <option value="Youngstown">Youngstown</option>
  <option value="Columbus">Columbus</option>
</select>
<button onclick="searchCSV()">Search</button>

<div id="result"></div>

<script>
async function searchCSV() {
  const selected = document.getElementById('categorySelect').value;
  const resultDiv = document.getElementById('result');
  resultDiv.textContent = '';

  if (!selected) {
    resultDiv.textContent = 'Please select a value.';
    return;
  }

  try {
    const response = await fetch('data.csv');
    if (!response.ok) throw new Error('Failed to load CSV.');
    const text = await response.text();

    // Parse CSV (assumes simple commas, no quotes)
    const rows = text.trim().split('\n').slice(1); // skip header
    const matches = rows
      .map(r => r.split(','))
      .filter(cols => cols[4] && cols[4].trim().toLowerCase() === selected.toLowerCase()) // 5th column index = 4
      .map(cols => cols[0].trim()); // first column

    resultDiv.innerHTML = matches.length
      ? `<strong>Matches:</strong> ${matches.join(', ')}`
      : 'No matches found.';
  } catch (err) {
    resultDiv.textContent = 'Error: ' + err.message;
  }
}
</script>
