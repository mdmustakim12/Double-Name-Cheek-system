<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8">
  <title>ডুপ্লিকেট নাম চেকার</title>
  <style>
    body { 
      font-family: sans-serif; 
      padding: 20px; 
      background: linear-gradient(135deg, #0d1117, #1a1f2e); 
      color: #e6edf3; 
    }
    h2 { 
      color: #ff9f1c; 
      text-align: center; 
      text-shadow: 1px 1px 3px #000; 
    }
    textarea { 
      width: 100%; 
      height: 180px; 
      background: #0d1117; 
      color: #f1f1f1; 
      border: 1px solid #444c56; 
      border-radius: 8px; 
      padding: 12px; 
      font-family: monospace; 
    }
    button { 
      width: 100%; 
      background: linear-gradient(90deg, #ff006e, #fb5607); 
      color: white; 
      border: none; 
      padding: 12px; 
      margin-top: 15px; 
      border-radius: 8px; 
      cursor: pointer; 
      font-size: 16px; 
      font-weight: bold; 
      transition: 0.3s; 
    }
    button:hover { 
      background: linear-gradient(90deg, #8338ec, #3a86ff); 
      transform: scale(1.05); 
    }
    #result { 
      margin-top: 20px; 
      background: #0d1117; 
      padding: 15px; 
      border-radius: 10px; 
      border: 1px solid #444c56; 
    }
    .dup-item { 
      margin-bottom: 15px; 
      padding: 12px; 
      background: linear-gradient(135deg, #1f2937, #2a2f45); 
      border-radius: 8px; 
      border-left: 5px solid #ff006e; 
      box-shadow: 0 0 10px rgba(255,0,110,0.4); 
    }
    .name { font-weight: bold; color: #3a86ff; font-size: 18px; }
    .count { color: #ffbe0b; font-size: 14px; }
    .numbers { color: #fb5607; font-size: 14px; display: block; margin-top: 5px; }
  </style>
</head>
<body>
  <h2>🌈 ডুপ্লিকেট নাম চেকার</h2>
  <textarea id="input" placeholder="লিস্ট পেস্ট করুন..."></textarea>
  <button onclick="check()">✨ চেক করো ✨</button>
  <div id="result"></div>

<script>
function check() {
    const text = document.getElementById('input').value.trim();
    if (!text) { alert("দয়া করে লিস্ট দিন!"); return; }

    const lines = text.split('\n');
    const nameToPos = new Map();

    lines.forEach(line => {
        const lineTrim = line.trim();
        if (!lineTrim) return;

        if (lineTrim.includes('➤')) {
            const parts = lineTrim.split('➤');
            const numberPart = parts[0].trim();   // আসল সিরিয়াল নম্বর
            const namePart = parts[1].trim().normalize(); // নাম (যে কোনো ভাষা/ইমোজি সহ)

            if (!nameToPos.has(namePart)) {
                nameToPos.set(namePart, []);
            }
            nameToPos.get(namePart).push(numberPart);
        }
    });

    let output = "<h3 style='color:#ff9f1c'>ডুপ্লিকেট ফলাফল:</h3>";
    let found = false;

    nameToPos.forEach((positions, name) => {
        if (positions.length > 1) {
            found = true;
            output += `
                <div class="dup-item">
                    <span class="name">${name}</span>
                    <span class="count"> — ${positions.length} বার আছে</span>
                    <span class="numbers">পজিশন: ${positions.join(', ')}</span>
                </div>`;
        }
    });

    document.getElementById('result').innerHTML = found ? output : "✅ কোনো ডুপ্লিকেট নাম পাওয়া যায়নি!";
}
</script>
</body>
</html>
