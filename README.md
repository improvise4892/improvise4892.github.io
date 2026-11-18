# improvise4892.github.io

<!DOCTYPE html>
<html>
<head>
  <title>Weight Tracker</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body { font-family: sans-serif; padding: 20px; max-width: 500px; margin: auto; }
    input, button { width: 100%; padding: 10px; margin: 5px 0; box-sizing: border-box; }
  </style>
</head>
<body>
  <h2>Weight & Calorie Tracker</h2>
  <input type="date" id="date">
  <input type="number" id="weight" placeholder="Weight (lbs)">
  <input type="number" id="calories" placeholder="Calories Consumed">
  <button onclick="addData()">Add Entry</button>
  <canvas id="chart"></canvas>

  <script>
    let entries = JSON.parse(localStorage.getItem('weightData')) || [];
    const ctx = document.getElementById('chart').getContext('2d');
    
    const chart = new Chart(ctx, {
      type: 'line',
      data: {
        labels: entries.map(e => e.date),
        datasets: [
          { label: 'Weight', data: entries.map(e => e.weight), borderColor: 'blue', fill: false },
          { label: 'Expected', data: entries.map(e => e.expected), borderColor: 'red', borderDash: [5, 5], fill: false }
        ]
      }
    });

    function addData() {
      const date = document.getElementById('date').value;
      const weight = parseFloat(document.getElementById('weight').value);
      const calories = parseFloat(document.getElementById('calories').value);
      
      if (!date || isNaN(weight)) return;

      // Simple expected weight calculation (assuming 2000 cal baseline, 3500 cal = 1 lb)
      const prevExpected = entries.length > 0 ? entries[entries.length - 1].expected : weight;
      const expected = prevExpected + ((calories - 2000) / 3500);

      entries.push({ date, weight, expected });
      localStorage.setItem('weightData', JSON.stringify(entries));
      
      chart.data.labels.push(date);
      chart.data.datasets[0].data.push(weight);
      chart.data.datasets[1].data.push(expected);
      chart.update();
    }
  </script>
</body>
</html>
