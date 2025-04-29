# Sewing Patterns

<div id="pattern-table"></div>

<!-- Load Tablesort library -->
<script src="https://unpkg.com/tablesort@5.2.1/dist/tablesort.min.js"></script>

<script>
// Define your MinIO URL
const MINIO_PATTERNS_URL = 'http://127.0.0.1:9000/sewing-data/sewing-patterns.csv';

// Fetch and render the table
fetch(MINIO_PATTERNS_URL)
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not OK');
    }
    return response.text();
  })
  .then(csvText => {
    const rows = csvText.trim().split('\n').map(line => line.split(','));
    const headers = rows[0];
    const dataRows = rows.slice(1);

    let html = '<table id="patterns-table" border="1" cellpadding="5" style="border-collapse: collapse; width: 100%; cursor: pointer;"><thead><tr>';
    headers.forEach(header => {
      html += `<th>${header}</th>`;
    });
    html += '</tr></thead><tbody>';

    dataRows.forEach(row => {
      html += '<tr>';
      row.forEach(cell => {
        html += `<td>${cell}</td>`;
      });
      html += '</tr>';
    });

    html += '</tbody></table>';

    document.getElementById('pattern-table').innerHTML = html;

    // Now make the table sortable
    new Tablesort(document.getElementById('patterns-table'));
  })
  .catch(error => {
    console.error('Error fetching or parsing CSV:', error);
    document.getElementById('pattern-table').innerHTML = '<p>Failed to load patterns.</p>';
  });
</script>